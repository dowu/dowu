<template>
  <main class="main">
    <Header :title="thought.title" :subtitle="{ date: thought.createdAt }" />
    <div class="container-outer">
      <hr />
      <nuxt-content :document="thought" />
      <hr />
    </div>
  </main>
</template>

<script>
import getMeta from "../../utils/getMeta";

export default {
  computed: {
    meta() {
      const metaData = {
        type: "article",
        title: this.thought.title,
        description: this.thought.subtitle,
        url: `${this.$config.baseUrl}/thoughts/${this.$route.params.slug}`,
        mainImage: this.thought.image,
      };
      return getMeta(metaData);
    },
  },
  head() {
    return {
      title: this.thought.title,
      meta: [
        ...this.meta,
        {
          property: "article:published_time",
          content: this.thought.createdAt,
        },
        {
          property: "article:modified_time",
          content: this.thought.updatedAt,
        },
      ],
      link: [
        {
          hid: "canonical",
          rel: "canonical",
          href: `https://dowu.xyz/thoughts${this.$route.params.slug}`,
        },
      ],
    };
  },

  async asyncData({ $content, params }) {
    const thought = await $content("thoughts", params.slug).fetch();
    return { thought };
  },
};
</script>

<style lang="scss" scoped>
.project-image {
  border-radius: 8px;
  height: 200px;
  width: 100%;
  overflow: hidden;
  @include flex(center, center, row);
}

::v-deep .nuxt-content {
  &-container {
    width: 100%;
  }

  & > * {
    &:not(img) {
      @include padding(0 32px);
    }
  }

  & > h1,
  & > h2,
  & > h3,
  & > h4,
  & > h5,
  & > h6 {
    a {
      display: none;
    }
  }
}
</style>
