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
      <div class="heading">
        Tag Example
      </div>
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

      <div>Other stuff</div>
    </div>
  </main>
</template>

<style>
  @media screen and (max-width: 375px) {
    main {
      margin: 0.5rem 0.5rem;

      header {
        .heading {
          font-size: 1.5rem;
          font-weight: bold;
        }
      }
    }
  }

  @media screen and (min-width: 768px) {
    main {
      margin: 1rem 2rem;
    }
  }
</style>
