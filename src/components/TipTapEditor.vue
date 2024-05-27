<template>
  <div>
    <form @submit.prevent="handleSubmit">
      <div>
        <input class="border-2 px-2 mb-3 rounded w-full disabled:bg-slate-200 text-sm" type="text" name="slug" id="slug"
          v-model="slugString" placeholder="/slug" disabled />
      </div>
      <div>
        <input class="border-2 p-4 mb-3 rounded w-full text-xl font-bold" type="text" name="title" id="title"
          v-model="title" placeholder="Title" />
      </div>
      <div v-if="editor" class="border-2 border-b-0 p-2 flex items-center justify-evenly flex-wrap">
        <button type="button" title="Unordered List" @click="editor.chain().focus().toggleBulletList().run()"
          :class="{ 'is-active': editor.isActive('bulletList') }">
          <Icon class="text-xl" icon="material-symbols:format-list-bulleted" />
        </button>
        <button type="button" title="Ordered List" @click="editor.chain().focus().toggleOrderedList().run()"
          :class="{ 'is-active': editor.isActive('orderedList') }">
          <Icon class="text-xl" icon="octicon:list-ordered" />
        </button>
        <button type="button" title="Code Snippet" @click="editor.chain().focus().toggleCode().run()"
          :disabled="!editor.can().chain().focus().toggleCode().run()"
          :class="{ 'is-active': editor.isActive('code') }">
          <Icon class="text-xl" icon="material-symbols:code-rounded" />
        </button>
        <button type="button" title="Code Block" @click="editor.chain().focus().toggleCodeBlock().run()"
          :class="{ 'is-active': editor.isActive('codeBlock') }">
          <Icon class="text-xl" icon="oui:editor-code-block" />
        </button>
        <button type="button" title="Blockquote" @click="editor.chain().focus().toggleBlockquote().run()"
          :class="{ 'is-active': editor.isActive('blockquote') }">
          <Icon class="text-xl" icon="tabler:blockquote" />
        </button>
        <button type="button" title="Reset to Paragraph" @click="editor.chain().focus().clearNodes().run()">
          <Icon class="text-xl" icon="uis:paragraph" />
        </button>
        <button type="button" title="Toggle Paragraph" @click="editor.chain().focus().setParagraph().run()"
          :class="{ 'is-active': editor.isActive('paragraph') }">
          <Icon class="text-xl" icon="material-symbols:format-paragraph-rounded" />
        </button>
        <button type="button" title="Left Paragraph Alignment"
          @click="editor.chain().focus().setTextAlign('left').run()"
          :class="{ 'is-active': editor.isActive({ textAlign: 'left' }) }">
          <Icon class="text-xl" icon="ci:text-align-left" />
        </button>
        <button type="button" title="Center Paragraph Alignment"
          @click="editor.chain().focus().setTextAlign('center').run()"
          :class="{ 'is-active': editor.isActive({ textAlign: 'center' }) }">
          <Icon class="text-xl" icon="ci:text-align-center" />
        </button>
        <button type="button" title="Right Paragraph Alignment"
          @click="editor.chain().focus().setTextAlign('right').run()"
          :class="{ 'is-active': editor.isActive({ textAlign: 'right' }) }">
          <Icon class="text-xl" icon="ci:text-align-right" />
        </button>
        <button type="button" title="Justify Paragraph Alignment"
          @click="editor.chain().focus().setTextAlign('justify').run()"
          :class="{ 'is-active': editor.isActive({ textAlign: 'justify' }) }">
          <Icon class="text-xl" icon="ci:text-align-justify" />
        </button>
        <button type="button" title="Horizontal Rule" @click="editor.chain().focus().setHorizontalRule().run()">
          <Icon class="text-xl" icon="octicon:horizontal-rule" />
        </button>
        <button type="button" title="Line Break" @click="editor.chain().focus().setHardBreak().run()">
          <Icon class="text-xl" icon="icon-park-outline:paragraph-break" />
        </button>
        <button type="button" title="Undo" class="disabled:text-slate-400" @click="editor.chain().focus().undo().run()"
          :disabled="!editor.can().chain().focus().undo().run()">
          <Icon class="text-xl" icon="material-symbols:undo" />
        </button>
        <button type="button" title="Redo" class="disabled:text-slate-400" @click="editor.chain().focus().redo().run()"
          :disabled="!editor.can().chain().focus().redo().run()">
          <Icon class="text-xl" icon="material-symbols:redo" />
        </button>
      </div>

      <!-- Bubble Menu -->
      <BubbleMenu v-if="editor" :tippy-options="{ duration: 100 }" :editor="editor"
        class="flex items-center gap-1 bg-slate-200 p-1 rounded">
        <button type="button" title="Bold" @click="editor.chain().focus().toggleBold().run()"
          :disabled="!editor.can().chain().focus().toggleBold().run()"
          :class="{ 'is-active': editor.isActive('bold') }">
          <Icon class="text-xl" icon="material-symbols:format-bold-rounded" />
        </button>
        <button type="button" title="Italic" @click="editor.chain().focus().toggleItalic().run()"
          :disabled="!editor.can().chain().focus().toggleItalic().run()"
          :class="{ 'is-active': editor.isActive('italic') }">
          <Icon class="text-xl" icon="material-symbols:format-italic-rounded" />
        </button>
        <button type="button" title="Underline" @click="editor.chain().focus().toggleUnderline().run()"
          :class="{ 'is-active': editor.isActive('underline') }">
          <Icon class="text-xl" icon="material-symbols:format-underlined-rounded" />
        </button>
        <button type="button" title="Strikethrough" @click="editor.chain().focus().toggleStrike().run()"
          :disabled="!editor.can().chain().focus().toggleStrike().run()"
          :class="{ 'is-active': editor.isActive('strike') }">
          <Icon class="text-xl" icon="bi:type-strikethrough" />
        </button>
        <button type="button" @click="editor.commands.toggleLink()" :class="{ 'is-active': editor.isActive('link') }">
          <Icon icon="material-symbols:add-link-rounded" class="text-xl" />
        </button>
        <button type="button" title="Toggle Highlight" @click="editor.chain().focus().toggleHighlight().run()"
          :class="{ 'is-active': editor.isActive('highlight') }">
          <Icon class="text-xl" icon="ant-design:highlight-twotone" />
        </button>
        <button type="button" title="Clear Marks" @click="editor.chain().focus().unsetAllMarks().run()">
          <Icon class="text-xl" icon="material-symbols:format-clear" />
        </button>
      </BubbleMenu>


      <!-- Floating Menu -->
      <FloatingMenu v-if="editor" :tippy-options="{ duration: 100 }" :editor="editor"
        class="flex items-center gap-1 bg-slate-200 p-1 rounded">

        <button type="button" title="Toggle Heading H1"
          @click="editor.chain().focus().toggleHeading({ level: 1 }).run()"
          :class="{ 'is-active': editor.isActive('heading', { level: 1 }) }">
          <Icon class="text-xl" icon="material-symbols:format-h1-rounded" />
        </button>
        <button type="button" title="Toggle Heading H2"
          @click="editor.chain().focus().toggleHeading({ level: 2 }).run()"
          :class="{ 'is-active': editor.isActive('heading', { level: 2 }) }">
          <Icon class="text-xl" icon="material-symbols:format-h2-rounded" />
        </button>
        <button type="button" title="Toggle Heading H3"
          @click="editor.chain().focus().toggleHeading({ level: 3 }).run()"
          :class="{ 'is-active': editor.isActive('heading', { level: 3 }) }">
          <Icon class="text-xl" icon="material-symbols:format-h3-rounded" />
        </button>
        <button type="button" title="Toggle Heading H4"
          @click="editor.chain().focus().toggleHeading({ level: 4 }).run()"
          :class="{ 'is-active': editor.isActive('heading', { level: 4 }) }">
          <Icon class="text-xl" icon="material-symbols:format-h4-rounded" />
        </button>
        <button type="button" title="Toggle Heading H5"
          @click="editor.chain().focus().toggleHeading({ level: 5 }).run()"
          :class="{ 'is-active': editor.isActive('heading', { level: 5 }) }">
          <Icon class="text-xl" icon="material-symbols:format-h5-rounded" />
        </button>
        <button type="button" title="Toggle Heading H6"
          @click="editor.chain().focus().toggleHeading({ level: 6 }).run()"
          :class="{ 'is-active': editor.isActive('heading', { level: 6 }) }">
          <Icon class="text-xl" icon="material-symbols:format-h6-rounded" />
        </button>
      </FloatingMenu>
      <EditorContent v-model="content" :editor="editor" />
      <div class="flex justify-end px-2 my-2">
        <Button type="submit" class="rounded-full" size="sm" :disabled="isSubmitting">
          <span v-if="isSubmitting">
            <Icon icon="line-md:loading-twotone-loop" class="text-xl w-4 h-4 mr-2" />
          </span>
          <span>{{ isSubmitting ? 'Saving...' : 'Create Post' }}</span>
        </Button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { useEditor, EditorContent, BubbleMenu, FloatingMenu } from '@tiptap/vue-3'
import StarterKit from '@tiptap/starter-kit'
import { Icon } from '@iconify/vue';
import Underline from '@tiptap/extension-underline'
import Highlight from '@tiptap/extension-highlight'
import TextAlign from '@tiptap/extension-text-align'
import Placeholder from '@tiptap/extension-placeholder'
import History from '@tiptap/extension-history'
import Link from '@tiptap/extension-link'
import Image from '@tiptap/extension-image'
import { computed, ref, watch } from 'vue'
import { Button } from '@/components/ui/button'
import axios from 'axios'
import { useToast } from '@/components/ui/toast/use-toast'


const content = ref('')
const title = ref('')
const isSubmitting = ref(false);
const { toast } = useToast()

const slugString = computed(() => {
  return title.value.split(' ').join('-').toLowerCase()
})

const editor = useEditor({
  extensions: [
    StarterKit.configure({
      heading: {
        levels: [1, 2, 3, 4, 5, 6]
      },
      history: false
    }),
    Underline,
    Highlight.configure({ multicolor: true }),
    TextAlign.configure({ types: ['heading', 'paragraph'] }),
    Placeholder.configure({
      placeholder: 'Write something...',
    }),
    Image,
    History,
    Link.configure({
      HTMLAttributes: {
        rel: 'noopener noreferrer',
        target: '_blank'
      },
      validate: href => /^https?:\/\//.test(href),
    })
  ],
  editorProps: {
    attributes: {
      class: 'prose lg:prose-sm max-w-4xl mx-auto p-4 border-2 rounded-b',
    },
  },
  onUpdate: ({ editor }) => {
    content.value = editor.getHTML()
  }
})

watch(editor, (newEditor) => {
  if (newEditor) {
    content.value = newEditor.getHTML();
  }
});
watch(title, (newTitle) => {
  if (newTitle) {
    title.value = newTitle;
  }
})
const handleSubmit = async () => {
  isSubmitting.value = true
  try {
    await axios.post('http://localhost:3000/save-content', { content: content.value, title: title.value, slug: slugString.value })
    toast({ description: 'Post created' })
  } catch (error) {
    console.error(error)
    toast(
      {
        title: 'Uh oh! Something went wrong.',
        description: 'There was a problem with your request.',
        variant: 'destructive',
      },
    )
  } finally {
    isSubmitting.value = false
  }
}
</script>

<style></style>