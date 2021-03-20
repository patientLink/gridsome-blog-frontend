<template>
  <Layout>
    <div class="project">

      <div class="container">

        <div class="project-header">
          <h1 class="project-title" v-html="$page.project.title" />
          <div class="project-info">

            <div class="categories-container">
              <div class="categories">
                <span class="label">Categories</span>
                <span 
                  class="category"
                  v-for="(category, index) in $page.project.categories" 
                  :key="index"
                  v-text="category.title"
                />
              </div>
            </div>

            <div class="year-container">
              <span class="label">Year</span>
              <div>{{$page.project.created_at | dateToYear}}</div>
              <!-- <div v-html="$page.project.created_at"/> -->
            </div>
          </div>
        </div>

        <div v-html="mdToHtml($page.project.content)" class="content" />

      </div>

    </div>
  </Layout>
</template>

<page-query>
query ($id: ID!) {
  project: strapiProject (id: $id) {
    title
    id
    created_at
    content
    categories {
      title
    }
    cover {
      url
    }
    bgColor
    fontColor
  }
}
</page-query>

<script>
import MarkdownIt from 'markdown-it'
const md = new MarkdownIt()

export default {
  metaInfo () {
    return {
      title: this.$page.project.title,
      bodyAttrs: {
        style: `background-color: ${this.$page.project.bgColor ? this.$page.project.bgColor : 'var(--color-base)'}; color: ${this.$page.project.fontColor ? this.$page.project.fontColor : 'var(--color-contrast)'}`
      }
    }
  },
  methods: {
    mdToHtml(markdown) {
      let html = md.render(markdown)
      let reg = /<img [^>]*src=['"]([^'"]+)[^>]*>/g
      let s = html.match(reg)
      if(!s) return html
      for(let i = 0; i < s.length; i++) {
        let srcImg = s[i].replace(reg, '$1');//取src面的内容
        if (srcImg.slice(0, 4) == 'http' || srcImg.slice(0, 5) == 'https') {
        //若src前4位置或者前5位是http、https则不做任何修改
          console.log('不做任何修改');
        } else {
        //修改富文本字符串内容 img标签src 相对路径改为绝对路径
          html = html.replace(new RegExp(srcImg, 'g'), this.GRIDSOME_API_URL + srcImg);
        }
      }
      return html
    }
  }
}
</script>

<style scoped>
.project-header {
  padding: 20vh 0 4rem 0;
}
.project-title {
  font-size: 4rem;
  margin: 0 0 4rem 0;
  padding: 0;
}
.project-info {
  display: flex;
  flex-wrap: wrap;
  font-size: 0.8rem;
}
.project-info > div {
  margin-right: 4rem;
}
.project-info > div:last-of-type {
  margin: 0;
}
.category:after {
  content: ', '
}
.category:last-of-type:after {
  content: '';
}
</style>
