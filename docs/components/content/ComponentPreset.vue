<template>
  <ContentRenderer :value="ast" />
</template>

<script setup lang="ts">
// @ts-expect-error
import { transformContent } from '@nuxt/content/transformers'

const props = defineProps({
  slug: {
    type: String,
    default: null
  }
})

const appConfig = useAppConfig()
const route = useRoute()
const slug = props.slug || route.params.slug[1]
const camelName = useCamelCase(slug)
const name = `U${useUpperFirst(camelName)}`

const preset = appConfig.ui[camelName]

const { data: ast } = await useAsyncData(`${name}-preset`, () => transformContent('content:_markdown.md', `
\`\`\`json
${JSON.stringify(preset, null, 2)}
\`\`\`\
`, {
  highlight: {
    theme: {
      light: 'material-lighter',
      dark: 'material-palenight'
    }
  }
}))
</script>
