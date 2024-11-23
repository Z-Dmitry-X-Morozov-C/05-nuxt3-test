<template>
  <div v-if="editor" class="container">
    <div class="conteiner-tools_group">
      <div
        class="conteiner-tools_group-undo tool-box"
        @click="editor.chain().focus().undo().run()"
        :disabled="!editor.can().chain().focus().undo().run()"
      >
        <ButtonUndo />
      </div>
      <div
        class="conteiner-tools_group-rendo tool-box"
        @click="editor.chain().focus().redo().run()"
        :disabled="!editor.can().chain().focus().redo().run()"
      >
        <ButtonRendo />
      </div>
      <div
        class="conteiner-tools_group-heading tool-box"
        @click="editor.chain().focus().toggleHeading({ level: 1 }).run()"
        :class="{ 'is-active': editor.isActive('heading', { level: 1 }) }"
      >
        <ButtonHeading />
      </div>
      <div
        class="conteiner-tools_group-paragraph tool-box"
        @click="editor.chain().focus().setParagraph().run()"
        :class="{ 'is-active': editor.isActive('paragraph') }"
      >
        <ButtonParagraph />
      </div>
      <div class="conteiner-tools_group-image tool-box" @click="addImage">
        <ButtonImage />
      </div>
    </div>
    <div class="conteiner-tools_group-copy_btn" @click="copyHTML">
      {{ copyBtn }}
    </div>
    <EditorContent :editor="editor" class="editor" />
  </div>
</template>

<script setup>
import { EditorContent, useEditor } from "@tiptap/vue-3";
import { StarterKit } from "@tiptap/starter-kit";
import { Image } from "@tiptap/extension-image";
import ButtonUndo from "./UI/ButtonUndo.vue";
import ButtonRendo from "./UI/ButtonRendo.vue";
import ButtonHeading from "./UI/ButtonHeading.vue";
import ButtonParagraph from "./UI/ButtonParagraph.vue";
import ButtonImage from "./UI/ButtonImage.vue";
import { ref } from "vue";

const copyBtn = ref("Скопировать HTML-код");

const editor = useEditor({
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
  <img src="https://sun9-62.userapi.com/impg/sl6oF8-XNm0_io5ZUqPc13O2s8-Fdg3j3y25jA/R-bhgKo-Zb0.jpg?size=739x308&quality=95&sign=899264cd623d48e551496acddfd9e168&type=album" alt="UserImage"/>
  `,
  extensions: [StarterKit, Image],
});

const addImage = () => {
  const url = window.prompt("Image URL");
  if (url) {
    editor.value.chain().focus().setImage({ src: url }).run();
    console.log(true);
  }
};

const copyHTML = () => {
  const htmlContent = editor.value.getHTML();

  navigator.clipboard
    .writeText(htmlContent)
    .then(() => {
      copyBtn.value = "Код скопирован!";

      setTimeout(() => {
        copyBtn.value = "Скопировать HTML";
      }, 1000);
    })
    .catch((err) => {
      console.error("Ошибка при копировании:", err);
    });
};

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

.conteiner-tools_group {
  display: flex;
  align-items: center;
  gap: 15px;
}

.conteiner-tools_group-copy_btn {
  color: #639eff;
  margin-top: 10px;
  cursor: pointer;
}

@media (max-width: 682px) {
  .conteiner-tools_group {
    display: flex;
    align-items: center;
    gap: 15px;
  }

  .conteiner-tools_group-copy_btn {
    display: inline-flex;
  }
}
</style>
