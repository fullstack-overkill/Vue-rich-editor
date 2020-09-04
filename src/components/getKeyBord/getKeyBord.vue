<template>
  <div class="select-shadow">
    <div class="select-cont">
      <div class="select-input">
        <div
          v-for="item in selectds"
          :key="item.name"
          class="select-option"
          :value="item.name"
          @click="addItensList({ $event })"
        >{{ item.name }}</div>
        <input
          type="text"
          @keydown.down="changeSelect({ $event }, 'dw')"
          @keydown.up="changeSelect({ $event }, 'up')"
        />
      </div>
      <div :class="{ 'select-opitions': true }">
        <div
          v-for="item in itensList"
          :key="item.name"
          :class="['list-obj', SelectId]"
          @click="addSelectds({ $event })"
          :value="item.name"
        >{{ item.name }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "getKeyBoard",
  data() {
    return {
      SelectId: "slec" + this._uid,
      ElementListAtual: 0,
      ElementListAnterior: 0,
      selectds: [],
      itensList: [
        { name: "node" },
        { name: "js" },
        { name: "js" },
        { name: "js" },
        { name: "js" },
      ],
      selectedElement: "",
    };
  },
  mounted() {},
  methods: {
    addSelectds($event) {
      let teste = $event.$event.target.getAttribute("value");
      this.selectds.push({ name: teste });
      $event.$event.target.remove();
      document.getElementsByClassName(this.SelectId)[this.ElementListAtual].classList.remove("list-select");
      this.ElementListAtual = 0;
      this.ElementListAnterior = 0;
    },
    addItensList($event) {
      let teste = $event.$event.target.getAttribute("value");
      this.itensList.push({ name: teste });
      $event.$event.target.remove();
      document.getElementsByClassName(this.SelectId)[this.ElementListAtual].classList.remove("list-select");
      this.ElementListAtual = 0;
      this.ElementListAnterior = 0;
    },
    changeSelect($event, Type) {
      $event;
      if (Type == "dw") {
        console.log(this.ElementListAtual)
        if (this.ElementListAtual != 0 && this.ElementListAnterior+1 != this.itensList.length) {
          document.getElementsByClassName(this.SelectId)[this.ElementListAnterior].classList.toggle("list-select");
          this.ElementListAnterior = this.ElementListAnterior + 1;
        }
        if (this.ElementListAtual+1 < this.itensList.length+1) {
          document.getElementsByClassName(this.SelectId)[this.ElementListAtual].classList.toggle("list-select");
          this.ElementListAtual = this.ElementListAtual + 1;
        }
      }
      if (Type == "up") {
          if (this.ElementListAtual != 0) {
            this.ElementListAtual = this.ElementListAtual - 1;
            document.getElementsByClassName(this.SelectId)[this.ElementListAtual].classList.toggle("list-select");
          }
          if (this.ElementListAtual != 0) {
            this.ElementListAnterior = this.ElementListAnterior - 1;
            document.getElementsByClassName(this.SelectId)[this.ElementListAnterior].classList.toggle("list-select");
          }
          
      }
    },
  },
};
</script>

<style scoped lang="stylus">
@import './getKeyBord.styl';
</style>
