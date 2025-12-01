<template>
  <header class="quiz-header shadow-sm">
    <b-container fluid>
      <b-row class="align-items-center py-3">
        
        <!-- Logo / Title -->
        <b-col cols="6" class="d-flex align-items-center">
          <h4 class="mb-0 app-title">🎯 Fancy Quiz App</h4>
        </b-col>

        <!-- Score Display -->
        <b-col cols="6" class="text-right">
          <span class="score-label">Score:</span>
          <b-badge variant="primary" pill class="score-badge">
            {{ numCorrect }}/{{ numTotal }}
          </b-badge>
        </b-col>

      </b-row>
    </b-container>
  </header>
</template>

<script>
export default {
  name: 'Header',
  props: ['numCorrect', 'numTotal']
}
</script>

<style scoped>
.quiz-header {
  background: #eef3ff;
  border-bottom: 2px solid #d9e2ff;
}

.app-title {
  font-weight: 600;
  color: #334;
}

.score-label {
  font-size: 1rem;
  font-weight: 500;
  margin-right: 6px;
}

.score-badge {
  font-size: 1rem;
  padding: 8px 14px;
}
</style>
