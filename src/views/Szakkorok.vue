<template>
  <div class="szakkorok-container">
    <div class="gyerekek-lista box">
      <h3 class="title">Gyerekek</h3>
      <ul>
        <!-- Gyerekek listája, amely a gyerekek tömbből generálódik -->
        <li 
          v-for="gyerek in gyerekek" 
          :key="gyerek.id" 
          @click="selectGyerek(gyerek)" 
          :class="{ selected: gyerek.id === selectedGyerek?.id }">
          {{ gyerek.nev }} ({{ gyerek.osztaly }})
        </li>
      </ul>
    </div>

    <!-- Elválasztó vonal -->
    <div class="separator"></div>

    <div class="szakkorok-lista box">
      <h3 class="title">Szakkörök</h3>
      <ul>
        <!-- Szakkörök listája -->
        <li 
          v-for="szakkor in szakkorok" 
          :key="szakkor.id" 
          @click="toggleGyerekInSzakkor(szakkor)"
          :class="{ active: szakkor.gyerekek.includes(selectedGyerek?.id) }">
          <span class="szakkor-nev">{{ szakkor.nev }}</span>
          <ul>
            <!-- Gyerekek listája, akik részt vesznek a szakkörben -->
            <li v-for="gyerekId in szakkor.gyerekek" :key="gyerekId">
              {{ gyerekek.find(gyerek => gyerek.id === gyerekId).nev }}
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Gyerekek adatai
      gyerekek: [
        { id: 1, nev: 'Kiss Anna', osztaly: '5A' },
        { id: 2, nev: 'Nagy Béla', osztaly: '6B' },
        { id: 3, nev: 'Tóth Gábor', osztaly: '4C' },
        { id: 4, nev: 'Szabó László', osztaly: '5B' },
        { id: 5, nev: 'Varga Emília', osztaly: '6C' },
        { id: 6, nev: 'Kovács Péter', osztaly: '4A' },
        { id: 7, nev: 'Kiss Zita', osztaly: '5A' },
        { id: 8, nev: 'Nagy Kálmán', osztaly: '6B' },
        { id: 9, nev: 'Tóth Éva', osztaly: '4C' },
        { id: 10, nev: 'Kiss András', osztaly: '5B' },
      ],
      // Szakkörök adatai
      szakkorok: [
        { id: 1, nev: 'Kosárlabda', gyerekek: [] },
        { id: 2, nev: 'Rajz', gyerekek: [] },
        { id: 3, nev: 'Foci', gyerekek: [] },
        { id: 4, nev: 'Zene', gyerekek: [] },
        { id: 5, nev: 'Tánc', gyerekek: [] },
        { id: 6, nev: 'Programozás', gyerekek: [] },
      ],
      // Kiválasztott gyerek
      selectedGyerek: null,
    };
  },
  methods: {
    // Kiválasztott gyerek beállítása
    selectGyerek(gyerek) {
      this.selectedGyerek = gyerek;
    },
    // Gyerek hozzáadása vagy eltávolítása a szakkörből
    toggleGyerekInSzakkor(szakkor) {
      if (!this.selectedGyerek) return; // Ha nincs kiválasztva gyerek, nem történik semmi

      const gyerekIndex = szakkor.gyerekek.indexOf(this.selectedGyerek.id);

      if (gyerekIndex === -1) {
        // Ha a gyerek még nincs a szakkörben, hozzáadjuk
        szakkor.gyerekek.push(this.selectedGyerek.id);
      } else {
        // Ha már benne van, eltávolítjuk
        szakkor.gyerekek.splice(gyerekIndex, 1);
      }
    },
  },
};
</script>

<style scoped>
.szakkorok-container {
  display: flex; /* Flexbox elrendezés a gyerekek és szakkörök számára */
  justify-content: space-between; /* Egyenlő távolság a dobozok között */
  padding: 20px; /* Belső margó */
  align-items: flex-start; /* Felső igazítás */
  background-color: var(--bg-black-100); /* Háttér szín */
}

.box {
  width: 45%; /* A doboz szélessége */
  padding: 20px; /* Belső margó */
  border: 1px solid rgba(255, 255, 255, 0.2); /* Határvonal */
  border-radius: 8px; /* Lekerekített sarkok */
  background-color: var(--bg-black-50); /* Háttér szín a dobozokban */
  box-shadow: 0 4px 20px var(--text-color); /* Árnyék hatás */
  transition: transform 0.3s, box-shadow 0.3s; /* Animáció az átalakulásokhoz */
}

.box:hover {
  transform: translateY(-3px); /* Felugrásos effektus hover esetén */
  box-shadow: 0 8px 40px rgba(0, 0, 0, 0.3); /* Tovább erősített árnyék hover esetén */
}

.separator {
  width: 2px; /* Elválasztó vonal szélessége */
  background-color: black; /* Elválasztó vonal színe */
  height: 100%; /* Elválasztó vonal magassága */
  margin: 0 20px; /* Külső margó */
}

ul {
  list-style-type: none; /* Lista pontok eltüntetése */
  padding: 0; /* Lista belső margó eltüntetése */
}

li {
  padding: 10px; /* Listaelem belső margó */
  cursor: pointer; /* Kéz kurzor a listaelem fölött */
  border-bottom: 1px solid #ddd; /* Listaelem alsó határvonal */
  transition: background-color 0.2s; /* Animáció a háttérszín változásához */
}

li:hover {
  background-color: var(--text-color); /* Hover hatás a háttérszínre */
}

li:last-child {
  border-bottom: none; /* Utolsó listaelem alsó határvonalának eltüntetése */
}

li.active {
  background-color: var(--text-color); /* Kiemelt háttérszín */
  color: white; /* Kiemelt szín */
}

.title {
  color: var(--text-color); /* Szín a címeknek */
  margin-bottom: 15px; /* Cím alatti margó */
}

.szakkor-nev {
  font-weight: bold; /* Kiemelt szakkör neve vastag */
  color: white; /* Kiemelt szakkör neve színe */
}

ul ul {
  margin-top: 10px; /* Gyereklista eltolása */
  padding-left: 20px; /* Gyereklista bal margó */
  color: white; /* Szín a gyerekek nevének */
}
</style>
