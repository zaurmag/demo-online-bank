<template>
  <app-page back title="Помощь">
    <question-item
        v-for="q in questions"
        :key="q.title"
        :question="q"
    ></question-item>
  </app-page>
</template>

<script>
import AppPage from '@/components/ui/AppPage'
import QuestionItem from '@/components/question/QuestionItem'

export default {
  setup () {
    const questions = [
      { title: 'Вопрос 1', text: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Delectus, harum!' },
      { title: 'Вопрос 2', text: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Delectus, harum!' },
      { title: 'Вопрос 3', text: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Delectus, harum!' }
    ]

    return {
      questions
    }
  },
  components: {
    AppPage,
    QuestionItem
  }
}
</script>
