<template>
    <div 
        class="journal-content"
        v-html="mdToHtml(content)"
    />
</template>

<script>
import MarkdownIt from 'markdown-it'
const md = new MarkdownIt()

export default {
    props: {
        content: String
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

<style>
p {
    line-height: 1.5;
    font-size: 1.15rem;
}
h2 {
    font-size: 2rem;
}
h3 {
    font-size: 1.5rem;
}
h4, h5, h6 {
    font-size: 1.15rem;
}
</style>
