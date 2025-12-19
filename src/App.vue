<template>
  <div class="app">
    <h1>Экспертная система подбора чая</h1>

    <div class="controls">
      <label>
        Горечь:
        <input type="range" min="1" max="5" v-model.number="prefs.bitterness"/>
        {{ prefs.bitterness }}
      </label>

      <label>
        Крепость:
        <input type="range" min="1" max="5" v-model.number="prefs.strength"/>
        {{ prefs.strength }}
      </label>

      <label>
        Кофеин:
        <input type="range" min="1" max="5" v-model.number="prefs.caffeine"/>
        {{ prefs.caffeine }}
      </label>
    </div>

    <button @click="recommend">Подобрать чай</button>

    <div v-if="result" class="result">
      <h2>Результат</h2>
      <p><strong>{{ result.name }}</strong></p>
      <p>Горечь: {{ result.bitterness }}</p>
      <p>Крепость: {{ result.strength }}</p>
      <p>Кофеин: {{ result.caffeine }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      teas: [
          //данные субъективные
        {id: 1, name: "Зелёный чай", bitterness: 2, strength: 2, caffeine: 2},
        {id: 2, name: "Чёрный чай", bitterness: 4, strength: 4, caffeine: 4},
        {id: 3, name: "Улун", bitterness: 3, strength: 3, caffeine: 3},
        {id: 4, name: "Белый чай", bitterness: 1, strength: 1, caffeine: 1},
        {id: 5, name: "Пуэр", bitterness: 5, strength: 5, caffeine: 3}
      ],
      prefs: {
        bitterness: 3,
        strength: 3,
        caffeine: 3,
        tolerance: 1
      },
      result: null
    };
  },

  methods: {
    recommend() {
      const t = this.prefs.tolerance;

      const matches = this.teas.filter(tea => {
        return (
            Math.abs(tea.bitterness - this.prefs.bitterness) <= t &&
            Math.abs(tea.strength - this.prefs.strength) <= t &&
            Math.abs(tea.caffeine - this.prefs.caffeine) <= t
        );
      });

      if (matches.length === 1) {
        this.result = matches[0];
      } else if (matches.length > 1) {
        this.result = matches[0];
      }
      // если не найдено
      else {
        this.result = null;
        alert("Подходящий чай не найден");
      }
    }
  }
};
</script>

<style>
.app {
  max-width: 500px;
  margin: 40px auto;
  font-family: Arial, sans-serif;
}

.controls label {
  display: block;
  margin-bottom: 15px;
}

button {
  margin-top: 20px;
  padding: 10px;
  width: 100%;
}

.result {
  margin-top: 30px;
  padding: 15px;
  border: 1px solid #ccc;
}
</style>
