<template>
  <Layout :global-content="globalData.content">
    <component
      v-if="story.content.component"
      :key="story.content._uid"
      :blok="story.content"
      :is="story.content.component"
    />
  </Layout>
</template>

<script>
import { getMetadataToPage } from '../utils/meta-tags'

export default {
  name: 'StoryblokEntryTemplate',
  metaInfo () {
    const config = {
      title: this.story.name,
      path: `/${this.story.full_slug}`
    }

    if (this.story.full_slug.indexOf('blog') !== -1) {
      config['description'] = this.story.content.summary
    }

    return getMetadataToPage(config)
  },
  computed: {
    story () {
      return this.$page.storyblokEntry
    },
    globalData () {
      return this.$page.global.edges[0].node
    }
  }
}
</script>

<page-query>
query StoryblokEntry ($id: ID) {
  storyblokEntry (id: $id) {
    id
    name
    slug
    full_slug
    content
  }

  global: allStoryblokEntry (filter: { slug: { eq: "global" } }) {
    edges {
      node {
        id
        full_slug
        content
      }
    }
  }
}
</page-query>
