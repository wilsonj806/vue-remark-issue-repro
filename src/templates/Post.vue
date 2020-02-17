<template>
  <div class="post">
    <!-- Markdown content slots in here -->
    <h1>{{ $page.post.title }}</h1>
    <VueRemarkContent/>
  </div>
</template>

<page-query>
query Post($id: ID!) {
  post(id: $id) {
    content
    title
  }
}
</page-query>

<script>
export default {

}
</script>

<style scoped>
.post {
  margin: 0 auto;
  max-width: 65%;
}
</style>