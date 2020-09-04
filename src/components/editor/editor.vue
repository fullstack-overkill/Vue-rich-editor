<template>
  <div class="editor-shadow">
    <div class="editor-paper">
      <div class="paper-container">
        <div class="paper-title">
          <input v-model="form.title" class="title" placeholder="Type a title..." type="text" />
        </div>
      </div>
      <div class="paper-tolltip">
        <div class="tolltip-container">
          <span
            class="material-icons icon-tolltip"
            v-on:click="
              editorComand('bold');
              toggleIcon({ $event });
            "
          >format_bold</span>
          <span
            class="material-icons icon-tolltip"
            @click="
              editorComand('italic');
              toggleIcon({ $event });
            "
          >format_italic</span>
          <span
            class="material-icons icon-tolltip"
            @click="
              editorComand('underline');
              toggleIcon({ $event });
            "
          >strikethrough_s</span>
          <span
            class="material-icons icon-tolltip"
            @click="
              editorComand('insertUnorderedList');
              toggleIcon({ $event });
            "
          >format_list_bulleted</span>
          <div>
            <span class="material-icons icon-tolltip disabled">link</span>
            <linkGenerator class="hide-el" />
          </div>
          <span
            class="material-icons icon-tolltip"
            @click="
              toggleIcon({ $event });
              toggleMark('backColor');
            "
          >format_color_reset</span>
          <span
            class="material-icons icon-tolltip"
            @click="
              toggleFont('formatBlock');
              toggleIcon({ $event });
            "
          >text_fields</span>
          <span
            class="material-icons icon-tolltip"
            @click="
              saveFile();
            "
          >cloud_download</span>
          <span
            class="material-icons icon-tolltip"
            @click="
              openLoadModal()
            "
          >cloud_upload</span>
        </div>
        <div class="tolltip-undo-redo">
          <span
            class="material-icons icon-tolltip"
            @click="
              editorComand('undo');
            "
          >undo</span>
          <span class="material-icons icon-tolltip disabled">redo</span>
        </div>
      </div>
      <input class="hide-el" type="file" id="file-input" @change="loadFile({ $event })" />
      <div id="contents"></div>
      <div
        class="paper-content"
        id="editor"
        oninput="if(this.innerHTML.trim()==='<br>')this.innerHTML=''"
        contenteditable
        placeholder="Write away..."
      ></div>
      <slot name="tt"></slot>
    </div>
  </div>
</template>

<script>
import linkGenerator from "./linkGenerator.vue";

export default {
  name: "Editor",
  components: {
    linkGenerator,
  },
  data() {
    return {
      textMark: false,
      textFont: false,
      form: {
        title: "",
      },
    };
  },
  methods: {
    openLoadModal(){
      document.getElementById("file-input").click()
    },
    loadFile($event) {
      // files that user has chosen
      var all_files = $event.$event.target.files;
      if (all_files.length == 0) {
        alert("Error : No file selected");
        return;
      }

      // first file selected by user
      var file = all_files[0];

      // file validation is successfull
      // we will now read the file

      var reader = new FileReader();

      // file reading started
      reader.addEventListener("loadstart", function () {
        document.querySelector("#file-input-label").style.display = "none";
      });

      // file reading finished successfully
      reader.addEventListener("load", function (e) {
        var text = e.target.result;

        // contents of the file
        document.querySelector("#editor").innerHTML = text;
        document.querySelector("#editor").style.display = "block";
      });

      // file reading failed
      reader.addEventListener("error", function () {
        alert("Error : Failed to read file");
      });

      // read as text file
      reader.readAsText(file);
    },
    saveFile() {
      var element = document.createElement("a");
      element.setAttribute(
        "href",
        "data:text/plain;charset=utf-8," +
          encodeURIComponent(document.getElementById("editor").innerHTML)
      );
      element.setAttribute("download", this.form.title + ".html");

      element.style.display = "none";
      document.body.appendChild(element);

      element.click();

      document.body.removeChild(element);
    },
    toggleFont() {
      let editableDocument = document.getElementById("editor");
      if (this.textMark == true) {
        document.execCommand("fontSize", false, "7");
      } else {
        document.execCommand("fontSize", false, "1");
      }

      editableDocument.focus();
    },
    toggleMark(sCmd) {
      let editableDocument = document.getElementById("editor");
      if (this.textMark == true) {
        document.execCommand(sCmd, false, "#FFDC00");
      } else {
        document.execCommand(sCmd, false, "#FFFFFF");
      }

      editableDocument.focus();
    },
    editorComand(sCmd, sValue) {
      let editableDocument = document.getElementById("editor");
      document.execCommand(sCmd, false, sValue);
      editableDocument.focus();
    },
    toggleIcon($event) {
      //document.execCommand("styleWithCSS", null, true);
      if ($event.$event.target.textContent == "format_color_reset") {
        if (this.textMark == false) {
          this.textMark = true;
        } else {
          this.textMark = false;
        }
      }
      if ($event.$event.target.textContent == "text_fields") {
        if (this.textFont == false) {
          this.textFont = true;
        } else {
          this.textFont = false;
        }
      }
      $event.$event.target.classList.toggle("icon-focus");
    },
    toggleLink() {
      document.getElementById("link").classList.toggle("hide-el");
    },
  },
};
</script>

<style scoped lang="stylus">
@import './editor.styl';
</style>
