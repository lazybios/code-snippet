<template>
    <section class="action">
        <a v-if="$root.getCodeExt(code.filename) == 'html'" @click.prevent="previewModal=true" 
        class="action-link" :href="`${$config.base.preview_url}/view/${snippet.id}/${code.filename}`"
        target="_blank" title="Preview">
            <Icon custom="fa fa-chrome"></Icon>
        </a>
        <a v-if="render" :class="{ ischeck: source }"
            title="View Source" @click="OnSource" 
            class="action-link" >
            <Icon custom="fa fa-code"></Icon>
        </a>
        <a title="Copy Code to Clipboard" class="action-link" 
            v-clipboard:copy="code.content"
            v-clipboard:success="onCopy"
            v-clipboard:error="onError">
            <Icon custom="fa fa-files-o"></Icon>
        </a>
        <a title="Download" @click="OnSave" class="action-link" >
            <Icon custom="fa fa-download"></Icon>
        </a>
        <Icon class="action-link execute" custom="fa fa-terminal" v-if="code.execute" title="Execute" @click="executeModal = true"></Icon>
        <Execute v-if="executeModal" v-model="executeModal" :snippet="code" :codes="[code]" :auto="false"></Execute>
        <Preview v-if="previewModal" v-model="previewModal" :snippet="snippet" :code="code.filename"></Preview>
    </section>
</template>

<script>
import { saveAs } from 'file-saver';
export default {
    name: 'Action',
    components: {
        Execute: () => import('./execute'),
        Preview: () => import('./preview')
    },
    props: {
        snippet: {
            required: true
        },
        code: {
            required: true
        },
        render: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            executeModal: false,
            previewModal: false,
            source: false
        }
    },
    computed: {
    },
    methods: {
        onCopy() {
            this.$Message.success('The code has been copied to your clipboard!');
        },
        onError(e) {
            this.$Message.error('Copy Failed:' + e.message);
        },
        OnSave() {
            saveAs(this.code.content, this.code.filename);
        },
        OnSource() {
            this.source = !this.source;
            this.$emit('source', this.source);
        }
    }
}
</script>

<style lang="less" scoped>
.action {
    padding: 0 .5em;
    line-height: 1.5;
}
.action-link {
    display: inline-block;
    padding: 0 5px;
}
.execute {
    cursor: pointer;
    &:hover {
        color: #2d8cf0;
    }
}
.ischeck {
    color: #19be6b;
    font-weight: bold;
    text-shadow: 0 0 2px #e3deb9;
}
</style>