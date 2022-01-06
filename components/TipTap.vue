<template>
  <div
    class="w-full overflow-hidden placeholder-gray-300 border border-gray-300 rounded-md shadow-sm ring-1 ring-transparent focus-within:ring-primary-500 focus-within:border-primary-500 sm:text-sm"
  >
    <div v-if="editor" class="tiptap__menu">
      <button :class="{ 'is-active': editor.isActive('bold') }" @click="editor.chain().focus().toggleBold().run()">
        bold
      </button>
      <button :class="{ 'is-active': editor.isActive('italic') }" @click="editor.chain().focus().toggleItalic().run()">
        italic
      </button>
      <button :class="{ 'is-active': editor.isActive('strike') }" @click="editor.chain().focus().toggleStrike().run()">
        strike
      </button>
      <button :class="{ 'is-active': editor.isActive('code') }" @click="editor.chain().focus().toggleCode().run()">
        code
      </button>
      <button @click="editor.chain().focus().unsetAllMarks().run()">clear marks</button>
      <button @click="editor.chain().focus().clearNodes().run()">clear nodes</button>
      <button
        :class="{ 'is-active': editor.isActive('paragraph') }"
        @click="editor.chain().focus().setParagraph().run()"
      >
        paragraph
      </button>
      <button
        :class="{ 'is-active': editor.isActive('heading', { level: 1 }) }"
        @click="editor.chain().focus().toggleHeading({ level: 1 }).run()"
      >
        h1
      </button>
      <button
        :class="{ 'is-active': editor.isActive('heading', { level: 2 }) }"
        @click="editor.chain().focus().toggleHeading({ level: 2 }).run()"
      >
        h2
      </button>
      <button
        :class="{ 'is-active': editor.isActive('heading', { level: 3 }) }"
        @click="editor.chain().focus().toggleHeading({ level: 3 }).run()"
      >
        h3
      </button>
      <button
        :class="{ 'is-active': editor.isActive('heading', { level: 4 }) }"
        @click="editor.chain().focus().toggleHeading({ level: 4 }).run()"
      >
        h4
      </button>
      <button
        :class="{ 'is-active': editor.isActive('heading', { level: 5 }) }"
        @click="editor.chain().focus().toggleHeading({ level: 5 }).run()"
      >
        h5
      </button>
      <button
        :class="{ 'is-active': editor.isActive('heading', { level: 6 }) }"
        @click="editor.chain().focus().toggleHeading({ level: 6 }).run()"
      >
        h6
      </button>
      <button
        :class="{ 'is-active': editor.isActive('bulletList') }"
        @click="editor.chain().focus().toggleBulletList().run()"
      >
        bullet list
      </button>
      <button
        :class="{ 'is-active': editor.isActive('orderedList') }"
        @click="editor.chain().focus().toggleOrderedList().run()"
      >
        ordered list
      </button>
      <button
        :class="{ 'is-active': editor.isActive('codeBlock') }"
        @click="editor.chain().focus().toggleCodeBlock().run()"
      >
        code block
      </button>
      <button
        :class="{ 'is-active': editor.isActive('blockquote') }"
        @click="editor.chain().focus().toggleBlockquote().run()"
      >
        blockquote
      </button>
      <button @click="editor.chain().focus().setHorizontalRule().run()">horizontal rule</button>
      <button @click="editor.chain().focus().setHardBreak().run()">hard break</button>
      <button @click="editor.chain().focus().undo().run()">undo</button>
      <button @click="editor.chain().focus().redo().run()">redo</button>
    </div>
    <editor-content :editor="editor" />
  </div>
</template>

<script>
import { Editor, EditorContent } from '@tiptap/vue-2'
import StarterKit from '@tiptap/starter-kit'

export default {
  components: {
    EditorContent
  },

  props: {
    value: { type: String, default: '' },
    type: { type: String, default: 'html' }
  },

  data() {
    return {
      editor: null
    }
  },

  watch: {
    value(value) {
      const isSame =
        this.type === 'html'
          ? this.editor.getHTML() === value // same for HTML format
          : JSON.stringify(this.editor.getJSON()) === JSON.stringify(value) // same for JSON format

      if (isSame) {
        return
      }
      this.editor.commands.setContent(value, false)
    }
  },

  mounted() {
    this.editor = new Editor({
      content: this.value,
      extensions: [StarterKit],
      editorProps: {
        attributes: {
          class: 'prose max-w-none focus:outline-none p-4'
        }
      },
      onUpdate: () => {
        const output = this.type === 'html' ? this.editor.getHTML() : this.editor.getJSON()
        this.$emit('input', output)
      }
    })
  },

  beforeDestroy() {
    this.editor.destroy()
  }
}
</script>

<style lang="postcss">
.tiptap__menu {
  @apply border-b border-gray-300;

  button {
    @apply p-2 border-gray-300;

    &.is-active {
      @apply bg-primary-400 text-white;
    }
  }
}
</style>
