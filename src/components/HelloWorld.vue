<template>
  <div class="container p-2 m-2 mt-5">
    <div class="row">
      <div
        class="col-md-6 mb-3"
        v-for="(item, index) in items.slice(0, 2)"
        :key="index"
        draggable="true"
        @dragstart="onDragStart($event, index)"
        @dragover="onDragOver($event)"
        @drop="onDrop($event, index)"
      >
        <div class="content-wrapper">
          <component :is="item.component"></component>
        </div>
      </div>
    </div>
    <div class="row mt-3">
      <div
        class="col-md-12 mb-3"
        draggable="true"
        @dragstart="onDragStart($event, 2)"
        @dragover="onDragOver($event)"
        @drop="onDrop($event, 2)"
      >
        <div class="content-wrapper">
          <component :is="items[2].component"></component>
        </div>
      </div>
    </div>
  </div>
  <hr class="border border-5 border-black">
  <div class="container">
    <svg
      @click="clearHighlight"
      @mouseover="clearHighlight"
      width="600"
      height="400"
      viewBox="0 0 600 400"
    >
      <g>
        <path
          v-for="area in areas"
          :key="area.id"
          :id="area.id"
          :d="area.d"
          :class="{ highlighted: area.id === highlightedArea }"
          @mouseover="highlightArea(area.id)"
          @click="showInfo(area)"
        />
      </g>
    </svg>
    <div v-if="selectedArea" class="info-box">
      <h3>{{ selectedArea.name }}</h3>
      <p>{{ selectedArea.description }}</p>
    </div>
  </div>
</template>

<script>
import ChartComponent1 from './ChartComponent1.vue';
import ChartComponent2 from './ChartComponent2.vue';
import TableComponent from './TableComponent.vue';

export default {
  components: {
    ChartComponent1,
    ChartComponent2,
    TableComponent,
  },
  data() {
    return {
      items: [
        { component: 'ChartComponent1', content: "Contenido 1" },
        { component: 'ChartComponent2', content: "Contenido 2" },
        { component: 'TableComponent', content: "Contenido 3" },
      ],
      draggedItemIndex: null,
      //data for svg interactive
      areas: [
        {
          id: "area1",
          name: "Área 1",
          description: "Descripción del Área 1",
          d: "M10 10 H 90 V 90 H 10 Z",
        },
        {
          id: "area2",
          name: "Área 2",
          description: "Descripción del Área 2",
          d: "M110 10 H 190 V 90 H 110 Z",
        },
        // Agrega más áreas según sea necesario
      ],
      highlightedArea: null,
      selectedArea: null,
    };
  },
  methods: {
   /**
   * Maneja el evento de inicio del arrastre.
   * @param {Event} event - El evento de arrastre.
   * @param {number} index - El índice del elemento arrastrado en el array items.
   */
  onDragStart(event, index) {
    // Guarda el índice del elemento que se está arrastrando
    this.draggedItemIndex = index;
    // Permite que el arrastre tenga un efecto de mover
    event.dataTransfer.effectAllowed = "move";
    // Asigna el índice del elemento arrastrado a los datos de transferencia del evento
    event.dataTransfer.setData("text/plain", index);
   
  },

  /**
   * Maneja el evento de arrastre sobre un elemento.
   * @param {Event} event - El evento de arrastre.
   */
  onDragOver(event) {
    // Previene el comportamiento por defecto para permitir el drop
    event.preventDefault();
    // Establece el efecto de arrastre a mover
    event.dataTransfer.dropEffect = "move";
  },

  /**
   * Maneja el evento de soltar un elemento.
   * @param {Event} event - El evento de soltar.
   * @param {number} index - El índice del elemento sobre el cual se suelta el elemento arrastrado.
   */
  onDrop(event, index) {
    // Previene el comportamiento por defecto
    event.preventDefault();
    // Obtiene el índice del elemento que se estaba arrastrando
    const draggedIndex = this.draggedItemIndex;

    // Verifica que el índice arrastrado no sea nulo y no sea igual al índice del elemento sobre el cual se suelta
    if (draggedIndex !== null && draggedIndex !== index) {
      // Intercambia los elementos en el array items
      const temp = this.items[draggedIndex];
      this.items.splice(draggedIndex, 1, this.items[index]);
      this.items.splice(index, 1, temp);

      // Reinicia el índice del elemento arrastrado
      this.draggedItemIndex = null;
    }
  },
    //methods for svg

    highlightArea(id) {
      this.highlightedArea = id;
    },
    showInfo(area) {
      this.selectedArea = area;
    },
    clearHighlight() {
      this.highlightedArea = null;
    },
  },
};
</script>

<style scoped>
.content-wrapper {
  border: 1px solid #ccc;
  padding: 20px;
  text-align: center;
  background-color: #f8f9fa;
  cursor: move;
  height: 100%; 
  display: flex;
  flex-direction: column;
  justify-content: center;
}


.component-inner {
  max-width: 100%; 
  overflow: hidden ;
}


path {
  fill: none;
  stroke: black;
  stroke-width: 2;
  transition: fill 0.3s;
}

path.highlighted {
  fill: yellow;
}

.info-box {
  margin-top: 20px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}
</style>

