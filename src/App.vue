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

		<!-- Train on randomised full SEEN set -->
		<Screen :title="'Training Round 1a'">
			When you are ready, click the button to start your training.
			<!-- for debugging -->
			<p> Chain: {{$magpie.socket.chain}} </p>
			<p> Generation: {{$magpie.socket.generation}} </p>
			<button @click="getPreviousResponse();$magpie.nextScreen();">
				Next
			</button>

		</Screen>

		<template v-for="(index, i) of train1a">
			<Screen :key = "i">
				<Slide> <!-- Display label for 1 second -->
					{{vocab[index]}}
					<Wait :time="1000" @done= "$magpie.nextSlide()" />
				</Slide>
				<Slide> <!-- Display picture and label for 5 seconds -->
					{{vocab[index]}}
					<br/>
<!-- 					{{index}}
					{{pics[index]}} -->
					<img :src="pics[index].pic" />
					<Record :data="{
						chain: $magpie.socket.chain,
						generation: $magpie.socket.generation,
						trial_type: 'training 1a',
						trial_number: i+1,
						expected: vocab[index],
						item: pics[index].item,
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
		<!-- Train on randomised full SEEN set -->
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
					<img :src="pics[index].pic" />
					<Record :data="{
						chain: $magpie.socket.chain,
						generation: $magpie.socket.generation,
						trial_type: 'training 1b',
						trial_number: i+1,
						expected: vocab[index],
						item: pics[index].item,
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
		<!-- Test on randomised halved SEEN and halved UNSEEN -->
		<InstructionScreen :title="'Testing Round 1'">
			When you are ready, click the button to start your test for training round 1.

		</InstructionScreen>

		<template v-for="(index, i) of test1">
			<Screen :key = "i">
				<Slide>
					<img :src="pics[index].pic" />
					<TextareaInput :response.sync="$magpie.measurements.input" />
					<button @click="$magpie.saveAndNextScreen();">Next</button>
					<Record :data="{
						chain: $magpie.socket.chain,
						generation: $magpie.socket.generation,
						trial_type: 'testing 1',
						trial_number: i+1,
						expected: vocab[index],
						item: pics[index].item,
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
					<img :src="pics[index].pic" />
					<Record :data="{
						chain: $magpie.socket.chain,
						generation: $magpie.socket.generation,
						trial_type: 'training 2a',
						trial_number: i+1,
						expected: vocab[index],
						item: pics[index].item,
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
					<img :src="pics[index].pic" />
					<Record :data="{
						chain: $magpie.socket.chain,
						generation: $magpie.socket.generation,
						trial_type: 'training 2b',
						trial_number: i+1,
						expected: vocab[index],
						item: pics[index].item,
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
					<img :src="pics[index].pic" />
					<TextareaInput :response.sync="$magpie.measurements.input" />
					<button @click="$magpie.saveAndNextScreen();">Next</button>
					<Record :data="{
						chain: $magpie.socket.chain,
						generation: $magpie.socket.generation,
						trial_type: 'testing 2',
						trial_number: i+1,
						expected: vocab[index],
						item: pics[index].item,
					}"/>
				</Slide>
			</Screen>
		</template>
		<!-- Round 3: seen and unseen -->
		<InstructionScreen :title="'Training Round 3a'">
			When you are ready, click the button to start your second round of training.

		</InstructionScreen>

		<template v-for="(index, i) of train3a">
			<Screen :key = "i">
				<Slide> <!-- Display label for 1 second -->
					{{vocab[index]}}
					<Wait :time="1000" @done= "$magpie.nextSlide()" />
				</Slide>
				<Slide> <!-- Display picture and label for 5 seconds -->
					{{vocab[index]}}
					<br/>
					<img :src="pics[index].pic" />
					<Record :data="{
						chain: $magpie.socket.chain,
						generation: $magpie.socket.generation,
						trial_type: 'training 3a',
						trial_number: i+1,
						expected: vocab[index],
						item: pics[index].item,
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

		<InstructionScreen :title="'Training Round 3b'">
			When you are ready, click the button to start your next training session.

		</InstructionScreen>

		<template v-for="(index, i) of train3b">
			<Screen :key = "i">
				<Slide> <!-- Display label for 1 second -->
					{{vocab[index]}}
					<Wait :time="1000" @done= "$magpie.nextSlide()" />
				</Slide>
				<Slide> <!-- Display picture and label for 5 seconds -->
					{{vocab[index]}}
					<br/>
					<img :src="pics[index].pic" />
					<Record :data="{
						chain: $magpie.socket.chain,
						generation: $magpie.socket.generation,
						trial_type: 'training 3b',
						trial_number: i+1,
						expected: vocab[index],
						item: pics[index].item,
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
		<InstructionScreen :title="'Final Testing'">
			When you are ready, click the button to start your final testing for the language.

		</InstructionScreen>

		<template v-for="(index, i) of final">
			<Screen :key = "i">
				<Slide>
					<img :src="pics[index].pic" />
					<TextareaInput :response.sync="$magpie.measurements.input" />
					<button @click="$magpie.saveAndNextScreen();">Next</button>
					<Record :data="{
						chain: $magpie.socket.chain,
						generation: $magpie.socket.generation,
						trial_type: 'final',
						trial_number: i+1,
						expected: vocab[index],
						item: pics[index].item,
					}"/>
				</Slide>
			</Screen>
		</template>
	 <PostTestScreen />

		<!-- While developing your experiment, using the DebugResults screen is fine,
			once you're going live, you can use the <SubmitResults> screen to automatically send your experimental data to the server. -->
		<!-- <DebugResultsScreen /> -->
		<SubmitResultsScreen />
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
import _filter from 'lodash/filter';
import _uniq from 'lodash/uniq';
import _at from 'lodash/at';

export default {
	name: 'App',
	data() {

		let pics = [];
		let vocab = [];
		let seen = [];
		let unseen = [];
		let train1a = [];
		let train1b = [];
		let test1 = [];
		let train2a = [];
		let train2b = [];
		let test2 = [];
		let train3a = [];
		let train3b = [];
		let final = [];

		return {
			pictures,
			pics,
			vocab,
			seen,
			unseen,
			train1a,
			train1b,
			test1,
			train2a,
			train2b,
			test2,
			train3a,
			train3b,
			final,
		};
	},

	created: function(){
		this.getPictures();
		this.generateVocab();
		this.splitSeenUnseen();
		this.prepareRound1();
		this.prepareRound2();
		this.prepareRound3();
	},

	methods: {

		getPictures:
		 function(){
			this.pics = pictures;
		 },

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

		sample_n: // samples n items from collection
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
		 function(){
		  // set this.vocab to unique, get corresponding indices
		  let unique_vocab = _uniq(this.vocab)
		  console.log(unique_vocab)
		  let indices = Array.from(unique_vocab, (el, _) => this.vocab.indexOf(el))
		  console.log(this.vocab)
		  console.log(indices)
			// reset seen and unseen
			console.log(indices.length)
			if (indices.length < num_seen){  // if too many ambiguous words
				num_seen = indices.length
				num_train = Math.ceil(num_seen / 2)
				num_test = Math.ceil((vocab_size - num_seen) / 2)
			}
			let sampled_unique_vocab_indices = this.sample_n(num_seen, indices)
			this.seen = _at(indices, sampled_unique_vocab_indices)
			this.unseen = _shuffle(_difference(Array.from(this.pictures, (_, idx) => idx), this.seen))
			// prepare data for each round
			this.prepareRound1();
			this.prepareRound2();
			this.prepareRound3();
		 },

		splitSeenUnseen:
		 function(){
			this.seen = this.sample_n(num_seen, this.vocab)
			let indices = Array.from(this.pictures, (_, idx) => idx)
			this.unseen = _shuffle(_difference(indices, this.seen))
		 },

		prepareRound1:
		 function(){
			this.train1a = _shuffle(this.seen)
			this.train1b = _shuffle(this.train1a)
			this.test1 = _shuffle(_slice(_shuffle(this.unseen), num_test).concat(_slice(_shuffle(this.seen), num_train)))
		 },

		prepareRound2:
		 function(){
			this.train2a = _shuffle(this.seen)
			this.train2b = _shuffle(this.train2a)
			this.test2 = _shuffle(_slice(_shuffle(this.unseen), num_test).concat(_slice(_shuffle(this.seen), num_train)))
		 },

		prepareRound3:
		 function(){
		 	this.train3a = _shuffle(this.seen)
			this.train3b = _shuffle(this.train3a)
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
			if (generation != 1){
				// retrieve data from previous iteration
				let lastIterationResults = this.$magpie.socket.lastIterationResults;
				console.log(lastIterationResults)
				// only data from the final round is relevant
				let filteredData = _filter(lastIterationResults, ['trial_type', 'final'])
				console.log(filteredData)

				// replace pics and vocab with previous results
				let items = filteredData.map(datapoint => datapoint.item - 1)
				this.pics = _at(this.pictures, items)
				this.vocab = filteredData.map(datapoint => datapoint.input)
				// filter
				this.filterAmbiguous();
			}
		}
	}
};

const syllables = ["ma", "ni", "so", "ru", "ka", "gu", "ti", "de", "pa"];
const vocab_size = 27;
const min_syllables = 2;
const max_syllables = 4;
let num_seen = Math.ceil(vocab_size / 2);
let num_train = Math.ceil(num_seen / 2);
let num_test = Math.ceil((vocab_size - num_seen) / 2);

</script>
