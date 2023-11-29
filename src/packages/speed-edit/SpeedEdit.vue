<template>
  <div class="_editor5-speed"></div>
</template>

<script>
import EditSpeed from "ckeditor5-speed";
export default {
  name: "SpeedEdit",
  model: {
    prop: "value",
  },
  components: {},
  props: {
    value: {
      type: String,
      default: "",
    },
    editConfig: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      defaultEditConfig: {},
      editor: null,
      inner: true,
    };
  },
  destroyed() {
    this.editor && this.editor.destroy().catch(console.error);
    this.editor = null;
  },
  mounted() {
    this.editor = EditSpeed.create(
      document.querySelector("._editor5-speed"),
      Object.assign(this.defaultEditConfig, this.editor)
    )
      .then((editor) => {
        this.$emit("then", editor);
        this.editor = editor;
        this.editor.setData(this.value);
        editor.model.document.on("change:data", () => {
          this.inner = false;
          this.$emit("input", editor.getData());
          this.$nextTick(() => {
            this.inner = true;
          });
        });
      })
      .catch((e) => console.error(e));
  },
  watch: {
    value(newVal) {
      if (this.inner) {
        this.editor.setData(newVal);
      }
    },
  },
  methods: {
    destroyEditor() {
      this.editor && this.editor.destroy().catch(console.error);
    },
  },
};
</script>

<style scoped></style>
