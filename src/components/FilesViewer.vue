<template>
    <table class="table">
        <tbody>
            <tr class="clickable" @click="$emit('back')">
                <td class="icon-row">
                    <IconFolderOpen class="icon-folder"/>
                </td>
                <td>...</td>
                <td></td>
            </tr>
            <tr v-for="file in files" :key="file.name" :class="{clickable : file.directory}"
            @click="onFileClick(file)">
                <td class="icon-row">
                    <IconFolder v-if="file.directory" class="icon-folder"/>
                    <IconFile v-else class="icon-file"/>
                </td>
                <td>{{file.name}}</td>
                <td><span class="float-end">{{file.size}}</span></td>
            </tr>
        </tbody>
    </table>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import IconFile from './IconFile.vue'
import IconFolder from './IconFolder.vue'
import IconFolderOpen from './IconFolderOpen.vue'

export default defineComponent({
    props : {
        files : {type : Array, default : () => []}
    },
    components: {IconFile, IconFolder, IconFolderOpen},
    setup (_, {emit}) {
        const onFileClick = file => {
            if (file.directory) emit('folderClick', file)
        }

        return {onFileClick}
    }
})
</script>

<style scoped>
.clickable {
  cursor: pointer;
}
.icon-row {
  width: 2em;
}
.icon-folder {
  width: 1em;
}
.icon-file {
  width: 0.75em;
}
</style>