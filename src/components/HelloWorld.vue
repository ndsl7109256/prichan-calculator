<template>
  <div class="container mt-5">
    <h1>分數計算器</h1>
    <div class="form-group">
      <label for="isLatest">最新:</label>
      <input type="checkbox" id="isLatest" v-model="isLatest" class="form-check-input">
    </div>
    <div class="form-group">
      <label for="originalScore">原始分數:</label>
      <input type="number" id="originalScore" v-model.number="originalScore" class="form-control">
    </div>
    <div class="form-group">
      <label for="category">服裝件數:</label>
      <select id="category" v-model.number="category" class="form-control">
        <option value="3">3</option>
        <option value="4">4</option>
      </select>
    </div>
    <div class="form-group">
      <label for="extraScore">額外加分:</label>
      <input type="number" id="extraScore" v-model.number="extraScore" class="form-control">
    </div>
    <div class="form-group">
      <label for="isRecommended">推薦風格:</label>
      <input type="checkbox" id="isRecommended" v-model="isRecommended" class="form-check-input">
    </div>
    <div class="form-group">
      <label for="type">稀有種類:</label>
      <select id="type" v-model="type" class="form-control">
        <option value="WR">WR</option>
        <option value="ER">ER/RR/FR</option>
        <option value="others">其他</option>
      </select>
    </div>
    <div class="form-group">
      <label for="songPeople">歌曲人數:</label>
      <select id="songPeople" v-model.number="songPeople" class="form-control">
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
        <option value="6">6</option>
      </select>
    </div>
    <div class="form-group">
      <label for="hasSmileCard">微笑友卡:</label>
      <input type="checkbox" id="hasSmileCard" v-model="hasSmileCard" class="form-check-input">
    </div>
    <div class="form-group">
      <button @click="calculateScore" class="btn btn-primary">計算分數</button>
    </div>
    <div class="result mt-3">
      <h2>服裝小計: {{ clothingSubtotal }}</h2>
      <h2>推薦團體: {{ recommendedGroup }}</h2>
      <h2>團體色彩: {{ groupColor }}</h2>
      <h2>團體設計: {{ groupDesign }}</h2>
      <h2>團體等級: {{ groupLevel }}</h2>
      <h2>友卡加成: {{ friendCardBonus }}</h2>
      <h2>總分: {{ totalScore }}</h2>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const isLatest = ref(false);
    const originalScore = ref(0);
    const category = ref(3);
    const extraScore = ref(0);
    const isRecommended = ref(false);
    const type = ref('WR');
    const songPeople = ref(2);
    const hasSmileCard = ref(false);
    const clothingSubtotal = ref(0);
    const recommendedGroup = ref(0);
    const groupColor = ref(0);
    const groupDesign = ref(0);
    const groupLevel = ref(0);
    const friendCardBonus = ref(0);
    const totalScore = ref(0);

    const calculateScore = () => {
      // Calculate clothing subtotal
      let subtotal = originalScore.value;

      if (isLatest.value) {
        subtotal += 200 * category.value;
      }

      if (isRecommended.value) {
        subtotal += 500;
      }

      if (type.value === 'WR') {
        subtotal += 900;
      } else if(type.value === 'ER'){
        subtotal += 600;
      } else {
        subtotal += 300
      }

      subtotal += extraScore.value;
      clothingSubtotal.value = subtotal;

      // Calculate friend card bonus
      const recommendedGroupScores = [700, 800, 900, 1000, 1100];
      const groupColorScores = [400, 500, 600, 700, 800];
      const groupDesignScores = [400, 500, 600, 700, 800];
      const groupLevelScores = [400, 500, 600, 700, 800];

      if (isRecommended.value) {
        recommendedGroup.value = recommendedGroupScores[songPeople.value - 2];
      }else{
        recommendedGroup.value = 0;
      }
      groupColor.value = groupColorScores[songPeople.value - 2];
      groupDesign.value = groupDesignScores[songPeople.value - 2];
      groupLevel.value = groupLevelScores[songPeople.value - 2];

      let friendCardSubtotal = recommendedGroup.value + groupColor.value + groupDesign.value + groupLevel.value;

      if (hasSmileCard.value) {
        friendCardSubtotal *= 2;
      }

      friendCardBonus.value = friendCardSubtotal;

      // Calculate total score
      totalScore.value = clothingSubtotal.value + friendCardBonus.value;
    };

    return {
      isLatest,
      originalScore,
      category,
      extraScore,
      isRecommended,
      type,
      songPeople,
      hasSmileCard,
      clothingSubtotal,
      recommendedGroup,
      groupColor,
      groupDesign,
      groupLevel,
      friendCardBonus,
      totalScore,
      calculateScore,
    };
  },
};
</script>

<style>
.container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
</style>