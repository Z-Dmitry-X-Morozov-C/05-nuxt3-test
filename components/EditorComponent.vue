<template>
  <div v-if="editor" class="container">
    <div class="container-tools__group">
      <div
        class="container-tools__group-undo tool-box"
        @click="editor.chain().focus().undo().run()"
        :disabled="!editor.can().chain().focus().undo().run()"
      >
        <ButtonTool>
          <UndoIcon />
        </ButtonTool>
      </div>
      <div
        class="container-tools__group-redo tool-box"
        @click="editor.chain().focus().redo().run()"
        :disabled="!editor.can().chain().focus().redo().run()"
      >
        <ButtonTool>
          <RedoIcon />
        </ButtonTool>
      </div>
      <div
        class="container-tools__group-heading tool-box"
        @click="editor.chain().focus().toggleHeading({ level: 1 }).run()"
        :class="{ 'is-active': editor.isActive('heading', { level: 1 }) }"
      >
        <ButtonTool>
          <HeadingIcon />
        </ButtonTool>
      </div>
      <div
        class="container-tools__group-paragraph tool-box"
        @click="editor.chain().focus().setParagraph().run()"
        :class="{ 'is-active': editor.isActive('paragraph') }"
      >
        <ButtonTool>
          <ParagraphIcon />
        </ButtonTool>
      </div>
      <div class="container-tools__group-image tool-box" @click="addImage">
        <ButtonTool>
          <ImageIcon />
        </ButtonTool>
      </div>
    </div>
    <div class="container-tools__group-copy_btn" @click="copyHTML">
      {{ copyBtn }}
    </div>
    <EditorContent :editor="editor" class="editor" />
  </div>
</template>

<script setup>
import { EditorContent, useEditor } from "@tiptap/vue-3";
import { StarterKit } from "@tiptap/starter-kit";
import { Image } from "@tiptap/extension-image";
import { ref } from "vue";
import ButtonTool from "./UI/ButtonTool.vue";
import RedoIcon from "./Icons/RedoIcon.vue";
import UndoIcon from "./Icons/UndoIcon.vue";
import HeadingIcon from "./Icons/HeadingIcon.vue";
import ParagraphIcon from "./Icons/ParagraphIcon.vue";
import ImageIcon from "./Icons/ImageIcon.vue";

const copyBtn = ref("Скопировать HTML - код");

const editor = useEditor({
  extensions: [StarterKit, Image],
  content: `
  <p>
    Таким образом консультация с широким активом представляет собой интересный эксперимент проверки позиций, 
    занимаемых участниками в отношении поставленных задач. С другой стороны постоянное информационно-пропагандистское 
    обеспечение нашей деятельности представляет собой интересный эксперимент проверки 
    форм развития. Идейные соображения высшего порядка, 
    а также укрепление и развитие структуры влечет за 
    собой процесс внедрения и модернизации 
    соответствующий условий активизации. Задача организации,
    в особенности же реализация намеченных плановых заданий 
    играет важную роль в формировании дальнейших направлений развития. 
    Повседневная практика показывает,
    что постоянное информационно-пропагандистское обеспечение
    нашей деятельности играет важную роль в формировании 
    существенных финансовых и административных условий.
  </p>
  <h1>Смотрите какие обезьянки</h1>
  <img src="https://sun9-62.userapi.com/impg/sl6oF8-XNm0_io5ZUqPc13O2s8-Fdg3j3y25jA/R-bhgKo-Zb0.jpg?size=739x308&quality=95&sign=899264cd623d48e551496acddfd9e168&type=album" alt="UserImage"/>`,
});

// Функция импорта изображения
function addImage() {
  const url = window.prompt("Image URL");
  if (url) {
    editor.value.chain().focus().setImage({ src: url }).run();
    console.log(true);
  }
}

// Функция копирования HTML кода
function copyHTML() {
  const htmlContent = editor.value.getHTML();

  navigator.clipboard
    .writeText(htmlContent)
    .then(() => {
      copyBtn.value = "Код скопирован!";

      setTimeout(() => {
        copyBtn.value = "Скопировать HTML";
      }, 1000);

      clearTimeout(copyBtn.value);
    })
    .catch((err) => {
      console.error("Ошибка при копировании:", err);
    });
}

onBeforeUnmount(() => {
  editor.destroy();
});
</script>

<style scoped>
.container {
  margin-top: 77px;
}

.tool-box {
  cursor: pointer;
}

.editor {
  margin-top: 31px;
}

.container-tools__group {
  display: flex;
  align-items: center;
  gap: 15px;
}

.container-tools__group-copy_btn {
  color: #639eff;
  margin-top: 10px;
  cursor: pointer;
}
</style>
