<template>
  <particles-bg type="circle" :bg="true" />
  <div class="container mt-2">
    <h4>{{path}}</h4>
    <div class="form-group mt-4 mb-2">
      <input v-model='searchString' class="form-control form-control-sm" placeholder="File search">
    </div>
    <FilesViewer :files="filteredFiles"  @back='back' @folderClick='open($event.name)'/>
  </div>
</template>

<script>
import FilesViewer from './components/FilesViewer.vue'
import fs from 'fs';
import pathModule from 'path';
import {computed, ref} from 'vue';
import {app} from '@electron/remote'
import { ParticlesBg } from "particles-bg-vue";


// convert size into readable units
const formatSize = size => {
  let i = Math.floor(Math.log(size)/ Math.log(1024))
  return (
    (size/ Math.pow(1024,i)).toFixed(2)* 1 + ' ' + ['B', 'kB', 'MB', 'GB', 'TB'][i]
  )
} 

export default {
  components : {FilesViewer, ParticlesBg},
  setup(){
    const path = ref(app.getAppPath())
    const files = computed(() => {
      // get file path
      const fileNames = fs.readdirSync(path.value)
      return fileNames
      // get file stats
        .map(file => {
          const stats = fs.statSync(pathModule.join(path.value, file))
          return {
            name : file,
            size : stats.isFile() ? formatSize(stats.size ?? 0 ): null,
            directory : stats.isDirectory()
          }
        })
        // sort files (Folder first)
        .sort((a,b) => {
          if (a.directory === b.directory) {
            return a.name.localeCompare(b.name)
          }
          return a.directory ? -1 : 1
        })
    })

    // naviagate back
    const back = () => {
      path.value = pathModule.dirname(path.value)
    }
    // open folder
    const open = folder => {
      path.value = pathModule.join(path.value, folder)
    }
    
    // Search
    const searchString = ref("")
    const filteredFiles = computed(() => {
      return searchString.value
        ? files.value.filter(s => s.name.startsWith(searchString.value))
        : files.value

    })

    return {path, open, back, files, searchString, filteredFiles}
  }
}
</script>

<style>

</style>
