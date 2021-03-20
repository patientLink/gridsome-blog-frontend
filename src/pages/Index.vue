<template>
  <Layout>
    <div class="container">
      <Hero />
      <ProjectsGrid :projects="$page.projects.edges" />
    </div>
    <LatestJournals :journals="$page.posts.edges" />
  </Layout>
</template>

<page-query>
query {
  posts: allStrapiPost {
    edges {
      node {
        title
        id
        content
        description
        author
        timeToRead
        created_at
        
      }
    }
  }

  projects: allStrapiProject {
    edges {
      node {
        title
        id
        author
        content
        created_at
        categories {
          title
        }
        cover {
          url
        }
      }
    }
  }

  main: allStrapiMain {
    edges {
      node {
        title
        subtitle
      }
    }
  }
}

</page-query>

<script>
import Hero from "@/components/Hero"
import ProjectsGrid from "@/components/ProjectsGrid"
import LatestJournals from "@/components/LatestJournals"

export default {
  name: 'IndexPage',
  metaInfo() {
    return {
      title: this.$page.main.edges[0].node.title
    }
    
  },
  components: {
    Hero,
    ProjectsGrid,
    LatestJournals
  }
}
</script>
