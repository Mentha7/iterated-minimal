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
		<InstructionScreen :title="'Training Round 1a'">
			When you are ready, click the button to start your training.

<!-- 			{{this.getPreviousResponse()}} -->

		</InstructionScreen>

		<template v-for="(index, i) of train1a">
			<Screen :key = "i">
				<Slide> <!-- Display label for 1 second -->
					{{vocab[index]}}
					<Wait :time="1000" @done= "$magpie.nextSlide()" />
				</Slide>
				<Slide> <!-- Display picture and label for 5 seconds -->
					{{vocab[index]}}
					<br/>
					<img :src="pictures[index].pic" />
					<Record :data="{
						trial_type: 'training 1a',
						trial_number: i+1,
						expected: vocab[index],
						item: pictures[index].item,
					}"/>
					<Wait :time="5000" @done= "$magpie.nextSlide()" />
<!--          <Wait :time="5000" @done= "$magpie.saveAndnextScreen()" /> -->
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

		<template v-for="(index, i) of train1b">
			<Screen :key = "i">
				<Slide> <!-- Display label for 1 second -->
					{{vocab[index]}}
					<Wait :time="1000" @done= "$magpie.nextSlide()" />
				</Slide>
				<Slide> <!-- Display picture and label for 5 seconds -->
					{{vocab[index]}}
					<br/>
					<img :src="pictures[index].pic" />
					<Record :data="{
						trial_type: 'training 1b',
						trial_number: i+1,
						expected: vocab[index],
						item: pictures[index].item,
					}"/>
					<Wait :time="5000" @done= "$magpie.nextSlide()" />
<!--          <Wait :time="5000" @done= "$magpie.saveAndnextScreen()" /> -->
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

		<template v-for="(index, i) of test1">
			<Screen :key = "i">
				<Slide>
					<img :src="pictures[index].pic" />
					<TextareaInput :response.sync="$magpie.measurements.input" />
					<button @click="$magpie.saveAndNextScreen();">Next</button>
					<Record :data="{
						trial_type: 'testing 1',
						trial_number: i+1,
						expected: vocab[index],
						item: pictures[index].item,
					}"/>
				</Slide>
			</Screen>
		</template>
		<!-- Round 2: halved_seen and halved_unseen -->
		<InstructionScreen :title="'Training Round 2a'">
			When you are ready, click the button to start your second round of training.

		</InstructionScreen>

		<template v-for="(index, i) of train2a">
			<Screen :key = "i">
				<Slide> <!-- Display label for 1 second -->
					{{vocab[index]}}
					<Wait :time="1000" @done= "$magpie.nextSlide()" />
				</Slide>
				<Slide> <!-- Display picture and label for 5 seconds -->
					{{vocab[index]}}
					<br/>
					<img :src="pictures[index].pic" />
					<Record :data="{
						trial_type: 'training 2a',
						trial_number: i+1,
						expected: vocab[index],
						item: pictures[index].item,
					}"/>
					<Wait :time="5000" @done= "$magpie.nextSlide()" />
<!--          <Wait :time="5000" @done= "$magpie.saveAndnextScreen()" /> -->
				</Slide>
				<Slide> <!-- Optional manual transition -->
					<button
						v-if="!$magpie.measurements.hasOwnProperty('response')"
						@click="$magpie.saveAndNextScreen();">Next</button>
				</Slide>
			</Screen>
		</template>

		<InstructionScreen :title="'Training Round 2b'">
			When you are ready, click the button to start your next training session.

		</InstructionScreen>

		<template v-for="(index, i) of train2b">
			<Screen :key = "i">
				<Slide> <!-- Display label for 1 second -->
					{{vocab[index]}}
					<Wait :time="1000" @done= "$magpie.nextSlide()" />
				</Slide>
				<Slide> <!-- Display picture and label for 5 seconds -->
					{{vocab[index]}}
					<br/>
					<img :src="pictures[index].pic" />
					<Record :data="{
						trial_type: 'training 2b',
						trial_number: i+1,
						expected: vocab[index],
						item: pictures[index].item,
					}"/>
					<Wait :time="5000" @done= "$magpie.nextSlide()" />
<!--          <Wait :time="5000" @done= "$magpie.saveAndnextScreen()" /> -->
				</Slide>
				<Slide> <!-- Optional manual transition -->
					<button
						v-if="!$magpie.measurements.hasOwnProperty('response')"
						@click="$magpie.saveAndNextScreen();">Next</button>
				</Slide>
			</Screen>
		</template>

		<InstructionScreen :title="'Testing Round 2'">
			When you are ready, click the button to start your test for training round 1.

		</InstructionScreen>

		<template v-for="(index, i) of test2">
			<Screen :key = "i">
				<Slide>
					<img :src="pictures[index].pic" />
					<TextareaInput :response.sync="$magpie.measurements.input" />
					<button @click="$magpie.saveAndNextScreen();">Next</button>
					<Record :data="{
						trial_type: 'testing 2',
						trial_number: i+1,
						expected: vocab[index],
						item: pictures[index].item,
					}"/>
				</Slide>
			</Screen>
		</template>
		<!-- Round 3: seen and unseen -->
		<InstructionScreen :title="'Final Testing'">
			When you are ready, click the button to start your final testing for the language.

		</InstructionScreen>

		<template v-for="(index, i) of final">
			<Screen :key = "i">
				<Slide>
					<img :src="pictures[index].pic" />
					<TextareaInput :response.sync="$magpie.measurements.input" />
					<button @click="$magpie.saveAndNextScreen();">Next</button>
					<Record :data="{
						trial_type: 'final',
						trial_number: i+1,
						expected: vocab[index],
						item: pictures[index].item,
					}"/>
				</Slide>
			</Screen>
		</template>
<!--    <PostTestScreen /> -->

		<!-- While developing your experiment, using the DebugResults screen is fine,
			once you're going live, you can use the <SubmitResults> screen to automatically send your experimental data to the server. -->
		<!-- <DebugResultsScreen /> -->
		<SubmitResults />
	</Experiment>
</template>

<script>
// Load data from csv files as javascript arrays with objects
import pictures from '../trials/init.csv';
// import _ from 'lodash'; // Import entire lodash library
import _random from 'lodash/random';
import _sample from 'lodash/sample';
import _shuffle from 'lodash/shuffle';
import _difference from 'lodash/difference';
import _slice from 'lodash/slice';

export default {
	name: 'App',
	data() {

		let vocab = [];
		let seen = [];
		let unseen = [];
		let train1a = [];
		let train1b = [];
		let test1 = [];
		let train2a = [];
		let train2b = [];
		let test2 = [];
		let final = [];

		return {
			pictures,
			vocab,
			seen,
			unseen,
			train1a,
			train1b,
			test1,
			train2a,
			train2b,
			test2,
			final,
		};
	},

	created: function(){
		this.generateVocab();
		this.splitSeenUnseen();
		this.prepareRound1();
		this.prepareRound2();
		this.prepareRound3();
	},

	methods: {

		generateVocab:
		 function(){
		 	// generate new vocab for each chain
			console.log(syllables);
			console.log(vocab_size);
			for (let i = 0; i < vocab_size; i++){
				let num_syllables = _random(min_syllables, max_syllables)
				let word = "";
				for (let j = 0; j < num_syllables; j++){
					word += _sample(syllables);
				}
				if (this.vocab.includes(word)) {
					i--; // duplicates not added to array
					console.log("created duplicate word");
				} else {
					this.vocab.push(word);
				}
			}

			console.log(this.vocab);
		 },

		sample_n:
		 function(n, collection){
		 	let results = [];
		 	let len = collection.length - 1;
		 	for (let i = 0; i < n; i++){
		 		let index = _random(len)
		 		if (results.includes(index)) {
		 			i--;
		 			console.log("chosen same index");
		 		} else{
		 			results.push(index);
		 		}
		 	}
		 	console.log(results);
		 	return results;
		 },

		filterAmbiguous:
		 function(){// set this.vocab to unique, get corresponding indices
		 	// reset seen and unseen
		 },

		splitSeenUnseen:
		 function(){
		 	this.seen = this.sample_n(num_seen, this.vocab)
		 	let indices = Array.from(this.pictures, (_, idx) => idx)
		 	this.unseen = _shuffle(_difference(indices, this.seen))
		 },

		prepareRound1:
		 function(){
		 	this.train1a = _slice(_shuffle(this.seen), num_train)
		 	this.train1b = _shuffle(this.train1a)
		 	this.test1 = _slice(_shuffle(this.unseen), num_test)
		 },

		prepareRound2:
		 function(){
		 	this.train2a = _slice(_shuffle(this.seen), num_train)
		 	this.train2b = _shuffle(this.train2a)
		 	this.test2 = _slice(_shuffle(this.unseen), num_test)
		 },

		prepareRound3:
		 function(){
		 	this.final = this.randomiseData(pictures)
		 },

		randomiseData:
		 function(some_array){
		 	let indices = Array.from(some_array, (_, idx) => idx)
		 	console.log(indices)
		 	return _shuffle(indices)
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

		//    var generation = this.$magpie.socket.generation;

		//    if (generation == 1){
		//      // if first generation in chain, read pairs from initial_pairs, randomise
		//      // return two/four arrays
		//      return;
		//    }

		//    var lastIterationResults = this.$magpie.socket.lastIterationResults;
		//    // lastIterationResults will be null before initialisation
		//    // if not first generation in chain nor empty socket, read from lastIterationResults
		//    // filter ambiguous pairs
		//    // move all but one of the ambiguous items to UNSEEN
		//    // randomise SEEN and UNSEEN
		//    // return two/four arrays
		//    return;
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
