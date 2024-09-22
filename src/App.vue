<script setup>
import { watch } from 'vue'
import { useQuery } from '@vue/apollo-composable'
import gql from 'graphql-tag'

// zapytania mozna trzymac w innych modulach
// przerobic zapytania aby byly ze zmiennymi i filtracja po datach
const GET_DATA = gql`
query {
  hdfData {
    x
    y
  }
}
`

const { result } = useQuery(GET_DATA)

watch(result, value => {
  console.log(value)
})

</script>

<template>
</template>

