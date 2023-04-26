<script
    lang="ts"
    setup
>
import Tags from './components/Tags.vue'
import { companyRepository } from './lib/companies/companyRepository'
import { ref } from 'vue'

const options = ref<Map<string, string>>(new Map<string, string>())
const tags = ref<string[]>(['AAPL', 'MSFT'])

const onQuery = (query: string = '') => {
  const results = companyRepository.search(query)

  const newOptions = new Map<string, string>()

  results.forEach((result) => {
    newOptions.set(result.ticker, `${result.ticker} ${result.name}`)
  })

  options.value = newOptions
}

const onSelected = (key: string) => {
  tags.value = [...tags.value, key]
  options.value = new Map<string, string>()
}

const onDeleted = (key: string) => {
  tags.value = tags.value.filter((tag) => tag !== key)
}
</script>

<template>
  <main>
    <header>
      <h1>
        Tag Example
      </h1>
    </header>

    <div class="demo">
      <Tags
        :options="options"
        :tags="tags"
        :placeholder="'Search for a company'"
        @query="onQuery"
        @selected="onSelected"
        @deleted="onDeleted"
      />
    </div>
  </main>
</template>

<style lang="scss">
  main {
    margin: 1rem 2rem;
  }
</style>
