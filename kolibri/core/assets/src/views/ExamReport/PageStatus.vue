<template>

  <KGrid class="page-status" :style="{ backgroundColor: $themeTokens.surface }">
    <KGridItem size="75" percentage>
      <div>
        <h1 class="title">
          <KLabeledIcon icon="person" :label="userName" />
        </h1>
        <KLabeledIcon icon="quiz" :label="$tr('title', { name: contentName })" />
      </div>

      <table class="scores">
        <tr>
          <th>
            {{ $tr('overallScore') }}
          </th>
          <td>
            <strong>
              {{ $formatNumber(score, { style: 'percent' }) }}
            </strong>
          </td>
        </tr>
        <tr>
          <th>
            {{ $tr('questionsCorrectLabel') }}
          </th>
          <td>
            {{ $tr('questionsCorrectValue', {
              correct: questionsCorrect, total: questions.length
            }) }}
          </td>
        </tr>
      </table>
    </KGridItem>
    <KGridItem size="25" percentage align="right">
      <div>
        <ProgressIcon class="svg-icon" :progress="progress" />
        <strong>
          {{ progressIconLabel }}
        </strong>
      </div>
      <div v-if="completed">
        <ElapsedTime :date="completionTimestamp" />
      </div>
    </KGridItem>
  </KGrid>

</template>


<script>

  import themeMixin from 'kolibri.coreVue.mixins.themeMixin';
  import KGrid from 'kolibri.coreVue.components.KGrid';
  import KGridItem from 'kolibri.coreVue.components.KGridItem';
  import ProgressIcon from 'kolibri.coreVue.components.ProgressIcon';
  import ElapsedTime from 'kolibri.coreVue.components.ElapsedTime';
  import KLabeledIcon from 'kolibri.coreVue.components.KLabeledIcon';
  import commonCoreStrings from 'kolibri.coreVue.mixins.commonCoreStrings';

  export default {
    name: 'PageStatus',
    components: {
      KGrid,
      KGridItem,
      ProgressIcon,
      ElapsedTime,
      KLabeledIcon,
    },
    mixins: [commonCoreStrings, themeMixin],
    props: {
      userName: {
        type: String,
        required: true,
      },
      questions: {
        type: Array,
        default: () => [],
      },
      completed: {
        type: Boolean,
        default: false,
      },
      completionTimestamp: { type: Date },
      contentName: {
        type: String,
        required: true,
      },
    },
    computed: {
      questionsCorrect() {
        return this.questions.reduce((a, q) => a + (q.correct === 1 ? 1 : 0), 0);
      },
      score() {
        return this.questions.reduce((a, q) => a + q.correct, 0) / this.questions.length || 0;
      },
      progress() {
        // Either return in completed or in progress
        return this.completed ? 1 : 0.1;
      },
      progressIconLabel() {
        if (this.completed) {
          return this.coreString('completedLabel');
        } else if (this.completed !== null) {
          return this.$tr('inProgress');
        } else {
          return this.$tr('notStartedLabel');
        }
      },
    },
    $trs: {
      title: `'{name}' performance`,
      overallScore: 'Overall score',
      questionsCorrectLabel: 'Questions correct',
      questionsCorrectValue: '{correct, number} out of {total, number}',
      inProgress: 'In progress',
      notStartedLabel: 'Not started',
    },
  };

</script>


<style lang="scss" scoped>

  .page-status {
    padding: 8px;
  }

  .svg-icon {
    margin-right: 8px;
    font-size: 1.3em;
  }

  .icon {
    position: relative;
    top: -2px;
  }

  .questions {
    margin-top: 10px;
  }

  .svg-item {
    display: inline-block;
    margin-right: 8px;
    vertical-align: middle;
  }

  .title {
    margin-top: 0;
  }

  .scores {
    min-width: 200px;
    margin-top: 24px;

    th {
      text-align: left;
    }

    th,
    td {
      height: 2em;
      padding-right: 24px;
      font-size: 14px;
    }
  }

</style>
