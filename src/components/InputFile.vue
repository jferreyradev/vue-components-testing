<script setup>
import { ref, computed } from 'vue'
import { useStore } from '../composables/useStore.js'

const file = ref()
const csvHeader = ref()
const csvContent = ref()

const { setGlobalState, setLocalState, addGlobalState, addLocalState, globalState, localState } = useStore()

//const store = useStore()

function removeItemArray(array, index) {
  return array.slice(0, index).concat(array.slice(index + 1))
}
function removeQuotes(string) {
  return string.replace(/"/g, '')
}

function readFile() {
  const reader = new FileReader()

  reader.onload = function () {
    console.log('on reader')
    csvContent.value = reader.result.trim()
    console.log(csvContent.value)
  }

  reader.onerror = (err) => console.log(err)
  reader.readAsText(file.value.files[0])

  reader.onloadend = function () {
    console.log('on load end')

    csvContent.value = csvContent.value.replace(/"/g, '').split('\n')
    csvHeader.value = csvContent.value[0].replace(/"/g, '').split(',')
    csvContent.value = csvContent.value.slice(1)
    //console.log(csvContent.value)
    //console.log(csvHeader.value)
    setGlobalState({ csvFile: { header: csvHeader.value, content: csvContent.value }})

    addGlobalState({agregados:{id:1,desc:"Prueba objeto"}})

  }
}

const cant = computed(() => {
  if (!csvContent.value) {
    return 0
  }
  return csvContent.value.length
})

const rows = computed(() => {
  if (!csvHeader.value) {
    return 0
  }
  return csvHeader.value.length
})
</script>

<template>
  <div>
    <input ref="file" type="file" id="file-selector" accept=".csv" @change="readFile()" />
  </div>
  <table v-if="globalState.csvFile" >
    <thead>
      <tr>
        <th v-for="(item, index) in globalState.csvFile.header" :key="index">{{ item }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in globalState.csvFile.content" :key="index">
        <td v-for="(item, index) in item.split(',')" :key="index">
          {{ item }}
        </td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <td :colspan="rows" v-show="cant > 0">Rows: {{ cant }}</td>
      </tr>
    </tfoot>
  </table>
  <div v-if="globalState.agregados">
    {{ globalState.agregados }}
  </div>
</template>

<style>
tr {
  border: 1px solid black;
}
th {
  background-color: #a4c5dc;
}
td {
  font-size: 10px;
  padding: 5px;
  border: 1px solid black;
}
tfoot tr td {
  background-color: #a4c5dc;
  text-align: end;
  font-size: 12px;
  font-weight: bold;
}
tr:hover {
  background-color: gray;
  color: white;
}
</style>
