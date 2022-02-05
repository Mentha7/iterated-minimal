<template>
  <Experiment title="_magpie demo">
    <InstructionScreen :title="'Welcome'">
      This is a sample introduction screen.
      <br />
      <br />
      This screen welcomes the participant and gives general information about
      the experiment.
      <br />
      <br />
      This mock up experiment is a showcase of the functionality of magpie.
    </InstructionScreen>

    <InstructionScreen :title="'General Instructions'">
      This is a sample instructions view.
      <br />
      <br />
      First you will go through two practice trials. The practice trial view
      uses magpie's forced choice trial input.
    </InstructionScreen>

    <!-- Here we create screens in a loop for every entry in forced_choice -->
    <template v-for="(forced_choice_task, i) of forced_choice">
      <ForcedChoiceScreen
        :key="'forcedchoice-' + i"
        :progress="i / forced_choice.length"
        :question="forced_choice_task.question"
        :options="[forced_choice_task.option1, forced_choice_task.option2]"
      >
        <template #stimulus>
          <img :src="forced_choice_task.picture" />
        </template>
      </ForcedChoiceScreen>
    </template>

    <!--

      Comment this in, to try out interactive components like the Chat component.

      <ConnectInteractiveScreen />

      <Screen>
          <Chat :messages.sync="$magpie.measurements.messages"></Chat>
          <button @click="$magpie.saveAndNextScreen()">Next</button>
      </Screen>

      -->

    <PostTestScreen />

    <!-- While developing your experiment, using the DebugResults screen is fine,
      once you're going live, you can use the <SubmitResults> screen to automatically send your experimental data to the server. -->
    <DebugResultsScreen />
  </Experiment>
</template>

<script>
// Load data from csv files as javascript arrays with objects
import forced_choice from '../trials/forced_choice.csv';
import _ from 'lodash';

export default {
  name: 'App',
  data() {
    return {
      forced_choice,
      // Expose lodash.range to template above
      range: _.range
    };
  }
};

</script>
