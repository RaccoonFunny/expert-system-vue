<template>
  <div class="container">
    <h1>Экспертная система подбора чая</h1>

    <div class="card">
      <h2>Ваши предпочтения</h2>

      <label>
        Горечь:
        <select v-model="facts.bitterness">
          <option value="notSet">Выбрать</option>
          <option value="low">Низкая</option>
          <option value="medium">Средняя</option>
          <option value="high">Высокая</option>
        </select>
      </label>

      <label>
        Крепость:
        <select v-model="facts.strength">
          <option value="notSet">Выбрать</option>
          <option value="low">Низкая</option>
          <option value="medium">Средняя</option>
          <option value="high">Высокая</option>
        </select>
      </label>

      <label>
        Кофеин:
        <select v-model="facts.caffeine">
          <option value="notSet">Выбрать</option>
          <option value="low">Низкий</option>
          <option value="medium">Средний</option>
          <option value="high">Высокий</option>
        </select>
      </label>

      <button @click="runExpertSystem">Подобрать чай</button>
    </div>

    <div v-if="results.length" class="result">
      <h2>Рекомендованные чаи:</h2>
      <ul>
        <li v-for="tea in results" :key="tea.name">{{ tea.name }}</li>
      </ul>
    </div>

    <div v-else-if="searched" class="result">
      <h2>Подходящие чаи не найдены.</h2>
      <p>Однако вот список близкий к запросу</p>
      <ul>
        <li v-for="tea in similar" :key="tea.name">{{ tea.name }} : <br>
          Гор: {{tea.bitterness}},<br>
          Креп: {{tea.strength}},<br>
          Коф: {{tea.caffeine}} <hr>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      facts: {
        bitterness: "notSet",
        strength: "notSet",
        caffeine: "notSet"
      },
      searched: false,
      results: [],
      similar: [],
      //GPT опасный список (дорого было бы всё пробовать и оценивать)
      teas: [
        { name: "Белый чай Бай Му Дань", bitterness: "low", strength: "low", caffeine: "low" },
        { name: "Белый чай Шоу Мэй", bitterness: "low", strength: "low", caffeine: "low" },
        { name: "Зелёный чай Лунцзин", bitterness: "low", strength: "medium", caffeine: "medium" },
        { name: "Зелёный чай Сенча", bitterness: "medium", strength: "medium", caffeine: "medium" },
        { name: "Зелёный чай Матча", bitterness: "high", strength: "high", caffeine: "high" },
        { name: "Жёлтый чай Хо Шань Хуан Я", bitterness: "low", strength: "medium", caffeine: "medium" },
        { name: "Улун Те Гуань Инь", bitterness: "medium", strength: "medium", caffeine: "medium" },
        { name: "Улун Да Хун Пао", bitterness: "high", strength: "high", caffeine: "medium" },
        { name: "Молочный улун", bitterness: "low", strength: "medium", caffeine: "medium" },
        { name: "Красный чай Дянь Хун", bitterness: "medium", strength: "high", caffeine: "high" },
        { name: "Ассам", bitterness: "high", strength: "high", caffeine: "high" },
        { name: "Цейлонский чёрный чай", bitterness: "medium", strength: "high", caffeine: "high" },
        { name: "Пуэр Шу", bitterness: "high", strength: "high", caffeine: "medium" },
        { name: "Пуэр Шэн", bitterness: "high", strength: "high", caffeine: "high" },
        { name: "Хэй Ча", bitterness: "medium", strength: "high", caffeine: "medium" },
        { name: "Габа улун", bitterness: "low", strength: "medium", caffeine: "low" },
        { name: "Ходзича", bitterness: "low", strength: "low", caffeine: "low" },
        { name: "Генмайча", bitterness: "low", strength: "medium", caffeine: "low" },
        { name: "Лапсанг Сушонг", bitterness: "high", strength: "high", caffeine: "high" },
        { name: "Травяной ройбуш", bitterness: "low", strength: "low", caffeine: "low" },
        { name: "Травяной каркаде", bitterness: "medium", strength: "medium", caffeine: "low" },
        { name: "Улун Дун Дин", bitterness: "medium", strength: "medium", caffeine: "medium" },
        { name: "Чёрный чай Кимун", bitterness: "medium", strength: "medium", caffeine: "high" },
        { name: "Зелёный чай Гёкуро", bitterness: "low", strength: "high", caffeine: "high" }
      ]
    };
  },
  methods: {
    runExpertSystem() {

      let corrects=0;
      this.results = [];
      this.similar = [];
      /** магия JS, позволит нам это сделать через фильтр, но ЕСЛИ ЭТО - ТО ТО*/
      this.teas.forEach((tea)=>{
        if (tea.bitterness === this.facts.bitterness || this.facts.bitterness === "notSet")corrects++;
        if (tea.caffeine === this.facts.caffeine || this.facts.caffeine === "notSet")corrects++;
        if (tea.strength === this.facts.strength || this.facts.strength === "notSet")corrects++;
        console.log(`${tea.name} имеет соответствие ${corrects}`)
        if (corrects===3) this.results.push(tea);
        if (corrects===2) this.similar.push(tea);
        corrects=0;
      })
      console.log("----------------------------------------")
      this.searched = true;
    }
  },
  watch: {
    facts:{
      handler(){
        this.runExpertSystem();
      },
      deep: true,
    }
  }
};
</script>

<style>
body {
  background: #f4f6f8;
  font-family: Arial, sans-serif;
}

.container {
  max-width: 500px;
  margin: 40px auto;
}

h1 {
  text-align: center;
}

.card {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 15px;
}

select {
  width: 100%;
  padding: 6px;
  margin-top: 5px;
}

button {
  width: 100%;
  padding: 10px;
  background: #3b82f6;
  border: none;
  color: white;
  font-size: 16px;
  border-radius: 6px;
  cursor: pointer;
}

button:hover {
  background: #2563eb;
}

.result {
  background: #ecfeff;
  padding: 15px;
  border-radius: 8px;
}
</style>
