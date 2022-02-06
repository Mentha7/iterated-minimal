<template>
	<Experiment title="Test Experiment">

		<InstructionScreen :title="'Welcome'">
			This is a sample introduction screen.
			<br />
			<br />
			This screen welcomes the participant and gives general information about
			the experiment.


		</InstructionScreen>

		<InstructionScreen :title="'General Instructions'">
			This is a sample instructions view.

		</InstructionScreen>

		<ConnectInteractiveScreen :title="'Connecting...'"></ConnectInteractiveScreen>

		<AwaitIteratedResultScreen :title="'Waiting for previous participant to finish'"></AwaitIteratedResultScreen>

		<!-- Round 1: halved_seen and halved_unseen -->
		<InstructionScreen :title="'Training Round 1'">
			When you are ready, click the button to start your training.

		</InstructionScreen>

		<template v-for="(trial, i) of train_pic">
			<Screen :key = "i">
				<Slide> <!-- Display label for 1 second -->
					{{train_vocab[i]}}
					<Wait :key="trial.label" :time="1000" @done= "$magpie.nextSlide()" />
				</Slide>
				<Slide> <!-- Display picture and label for 5 seconds -->
					{{train_vocab[i]}}
					<br/>
					<img :src="trial.pic" />
					<!-- <TextareaInput :response.sync="$magpie.measurements.input" /> -->
					<!-- <button @click="$magpie.saveAndNextScreen();">Next</button> -->
					<Record :data="{
						trial_type: 'training 1a',
						trial_number: i+1,
						expected: train_vocab[i],
						item: trial.item,
					}"/>
					<Wait :key="trial.label" :time="5000" @done= "$magpie.nextSlide()" />
<!-- 					<Wait :key="trial.label" :time="5000" @done= "$magpie.saveAndnextScreen()" /> -->
				</Slide>
				<Slide> <!-- Optional manual transition -->
					<button
						v-if="!$magpie.measurements.hasOwnProperty('response')"
						@click="$magpie.saveAndNextScreen();">Next</button>
				</Slide>
			</Screen>
		</template>

		<InstructionScreen :title="'Training Round 1b'">
			When you are ready, click the button to start your next training session.

		</InstructionScreen>

		<template v-for="(trial, i) of train_pic">
			<Screen :key = "i">
				<Slide> <!-- Display label for 1 second -->
					{{train_vocab[i]}}
					<Wait :key="trial.label" :time="1000" @done= "$magpie.nextSlide()" />
				</Slide>
				<Slide> <!-- Display picture and label for 5 seconds -->
					{{train_vocab[i]}}
					<br/>
					<img :src="trial.pic" />
					<!-- <TextareaInput :response.sync="$magpie.measurements.input" /> -->
					<!-- <button @click="$magpie.saveAndNextScreen();">Next</button> -->
					<Record :data="{
						trial_type: 'training 1b',
						trial_number: i+1,
						expected: train_vocab[i],
						item: trial.item,
					}"/>
					<Wait :key="trial.label" :time="5000" @done= "$magpie.nextSlide()" />
<!-- 					<Wait :key="trial.label" :time="5000" @done= "$magpie.saveAndnextScreen()" /> -->
				</Slide>
				<Slide> <!-- Optional manual transition -->
					<button
						v-if="!$magpie.measurements.hasOwnProperty('response')"
						@click="$magpie.saveAndNextScreen();">Next</button>
				</Slide>
			</Screen>
		</template>

		<InstructionScreen :title="'Testing Round 1'">
			When you are ready, click the button to start your test for training round 1.

		</InstructionScreen>

		<template v-for="(trial, i) of test_pic">
			<Screen :key = "i">
				<Slide> <!-- Display picture and label for 5 seconds -->
					<img :src="trial.pic" />
					<TextareaInput :response.sync="$magpie.measurements.input" />
					<button @click="$magpie.saveAndNextScreen();">Next</button>
					<Record :data="{
						trial_type: 'testing 1',
						trial_number: i+1,
						expected: test_vocab[i],
						item: trial.item,
					}"/>
				</Slide>
			</Screen>
		</template>
		<!-- Round 2: halved_seen and halved_unseen -->

		<!-- Round 3: seen and unseen -->

<!-- 		<PostTestScreen /> -->

		<!-- While developing your experiment, using the DebugResults screen is fine,
			once you're going live, you can use the <SubmitResults> screen to automatically send your experimental data to the server. -->
		<DebugResultsScreen />
	</Experiment>
</template>

<script>
// Load data from csv files as javascript arrays with objects
import pictures from '../trials/init.csv';
// import _ from 'lodash'; // Import entire lodash library
import _random from 'lodash/random';
import _sample from 'lodash/sample';
import _shuffle from 'lodash/shuffle';
import _pull from 'lodash/pull';
import _slice from 'lodash/slice';

export default {
	name: 'App',
	data() {
		// generate new vocab for each chain
		console.log(syllables);
		console.log(vocab_size);
		let vocab = [];
		for (let i = 0; i < vocab_size; i++){
			let num_syllables = _random(min_syllables, max_syllables)
			let word = "";
			for (let j = 0; j < num_syllables; j++){
				word += _sample(syllables);
			}
			if (vocab.includes(word)) {
				i--; // duplicates not added to array
				console.log("created duplicate word");
			} else {
				vocab.push(word);
			}
		}

		console.log(vocab);

		const seen_vocab = _slice(_shuffle(vocab), num_seen - 1);
		const seen_pic = _slice(_shuffle(pictures), num_seen - 1);

		const unseen_vocab = _pull(vocab, ...seen_vocab);
		const unseen_pic = _pull(pictures, ...seen_pic);

		console.log(seen_pic);
		console.log(seen_vocab);
		console.log(unseen_pic);
		console.log(unseen_vocab);

		const train_vocab = _slice(_shuffle(seen_vocab), num_train);
		const test_vocab = _slice(_shuffle(unseen_vocab), num_test);

		let train_pic = [];
		let test_pic = [];

		for (const el of train_vocab) {
			train_pic.push(seen_pic[seen_vocab.indexOf(el)]);
		}

		for (const el of test_vocab) {
			test_pic.push(unseen_pic[unseen_vocab.indexOf(el)]);
		}

		console.log(train_vocab);
		console.log(train_pic);
		console.log(test_vocab);
		console.log(test_pic);


		return {
			pictures,
			vocab,
			seen_pic,
			seen_vocab,
			unseen_pic,
			unseen_vocab,
			train_vocab,
			test_vocab,
			train_pic,
			test_pic,
			// Expose lodash.method to template above
			// range: _.range
		};
	},
	methods: {
		randomiseData:
		 function(vocab_array, pic_array){
		 	let shuffled_vocab = _shuffle(vocab_array);
		 	let matching_pics = [];
		 	for (const el of shuffled_vocab) {
			matching_pics.push(pic_array[vocab_array.indexOf(el)]);
		}
		 console.log(shuffled_vocab);
		 console.log(matching_pics);

		 	return {
		 		shuffled_vocab,
		 		matching_pics,
		 	}
		 },

		getPreviousResponse:
		 function(){
		 	let generation = this.$magpie.socket.generation;
		 	console.log(generation);
		 	if (generation == null){
		 		return "empty socket";
		 	}
		 	if (generation == 1){
		 		// if first generation in chain, read pairs from initial_pairs, randomise
		 		// return two/four arrays
		 		return 0;
		 	} else{
		 		return -1;
		 	}
		 }
		// getPreviousResponse:
		//  function(){

		//  	var generation = this.$magpie.socket.generation;

		//  	if (generation == 1){
		//  		// if first generation in chain, read pairs from initial_pairs, randomise
		//  		// return two/four arrays
		//  		return;
		//  	}

		//  	var lastIterationResults = this.$magpie.socket.lastIterationResults;
		//  	// lastIterationResults will be null before initialisation
		//  	// if not first generation in chain nor empty socket, read from lastIterationResults
		//  	// filter ambiguous pairs
		//  	// move all but one of the ambiguous items to UNSEEN
		//  	// randomise SEEN and UNSEEN
		//  	// return two/four arrays
		//  	return;
		//  }
	}
};

const syllables = ["ma", "ni", "so", "ru", "ka", "gu", "ti", "de", "pa"];
const vocab_size = 27;
const min_syllables = 2;
const max_syllables = 4;
const num_seen = 14;
const num_train = 7;
const num_test = 7;

</script>
