<template>
  <div class="">
    <div class="">
      <Hero/>
      <TechStack/>
    </div>

    <Blogs :posts="posts" title="Featured Projects & Stories"/>
    <Projects :projects="projects"/>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios, $notion, $config: { githubUsername, notionTableId } }) {
    const projects = await $axios
      .get(
        'https://api.github.com/search/repositories?q=user:' + githubUsername + '&sort=stars&per_page=3'
      )
      .catch((errors) => {
        // console.log(errors)
      })
    const pageTable = await $notion.getPageTable(notionTableId)
    const posts = pageTable.filter((page) => page.public).sort((a, b) => new Date(b.created_at) - new Date(a.created_at))
    return { posts, projects: projects.data.items }
  }
}
</script>
