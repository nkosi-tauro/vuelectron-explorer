<template>
    <div>

    </div>
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
        

        return {}
    }
})
</script>

<style scoped>

</style>