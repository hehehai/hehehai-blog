<template>
  <div class="post-meta">
    <span class="time">{{ post.date }}</span>
    <template v-if="post.timeToRead">
      <strong>阅读时长 {{ post.timeToRead }}分钟</strong>
    </template>
  </div>
</template>

<script>
export default {
  props: ["post"]
};
</script>

<style lang="scss">
.post-meta {
  font-size: 0.8em;
  opacity: 0.8;

  .time {
    margin-right: 1em;
  }
}
</style>
