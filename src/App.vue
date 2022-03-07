<template>
	<Experiment title="Test Experiment">

		<InstructionScreen :title="'Welcome!'">
		Thank you very much for taking part in our experiment! Your participation is completely voluntary and you may quit at any time.
		<br>
		<br>
		Personal data collected in this experiment is anonymized and will be used for research purposes only.


		</InstructionScreen>

		<InstructionScreen :title="'General Instructions'">
			In this experiment, you will learn a few new words of a novel language. The whole experiment consists of three rounds of vocabulary learning. A single learning round is composed of two training sessions and one testing session.
			<br>
			<br>
			In training sessions, you'll first see the word you're supposed to learn, followed by a <b>pictorial representation</b> of it with no additional explanations, like the image shown below:
			<br>
			<img :src="pics[18].pic" />
			<br>
			You have <b>5 seconds</b> to learn each word-picture pair. When time runs out, the visual cues will disappear and you may move on to the next word when you're ready.
			<br>
			<br>
			In the testing sessions, only pictures (like the one above) will be displayed on the screen. You need to recall words (or invent some when necessary) associated with those pictures and type in your answer.
			<br>
			<br>
			In short, your task is to learn a few words with the aid of visual representations of their meanings (and nothing else) and then demonstrate your learning result in testing phases.
			<br>
			<br>
			The experiment will take approximately <b>20 minutes</b> to go through. We encourage you to work through it in one sitting with full concentration and avoid external distractions as much as possible. However, you can rest assured that you'll not be judged by your performance. Just take it easy and have fun :-)
			<br>
			<br>
			If you're prepared, click the button below to begin the experiment.

		</InstructionScreen>

		<ConnectInteractiveScreen :title="'Connecting...'"></ConnectInteractiveScreen>

		<AwaitIteratedResultScreen :title="'Waiting for previous participant to finish'"></AwaitIteratedResultScreen>

		<!-- Train on randomised full SEEN set -->
		<Screen :title="'Training Round 1a'">
			When you are ready, click the button to start your first training session.
			<!-- for debugging -->
<!--       <p> Chain: {{$magpie.socket.chain}} </p>
			<p> Generation: {{$magpie.socket.generation}} </p> -->
			<button @click="getPreviousResponse();shuffleSeen();$magpie.nextScreen();">
				Next
			</button>

		</Screen>

		<template v-for="(index, i) of train">
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
		<Screen :title="'Training Round 1b'">
			Was that too hard? Don't worry, you'll get used to it with more training.
			<br>
			<br>
			When you are ready, click the button to start your next training session.
			<button @click="shuffleSeen();$magpie.nextScreen();">
				Next
			</button>
		</Screen>

		<template v-for="(index, i) of train">
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
		<Screen :title="'Testing Round 1'">
			Congrats for completing the first round of training!
			<br>
			<br>
			When you are ready, click the button to start your test session.
			<button @click="prepareTest();$magpie.nextScreen();">
				Next
			</button>
		</Screen>

		<template v-for="(index, i) of test">
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
		<Screen :title="'Training Round 2a'">
			When you are ready, click the button to start your second round of training.
			<button @click="shuffleSeen();$magpie.nextScreen();">
				Next
			</button>
		</Screen>

		<template v-for="(index, i) of train">
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

		<Screen :title="'Training Round 2b'">
			When you are ready, click the button to start your next training session.
			<button @click="shuffleSeen();$magpie.nextScreen();">
				Next
			</button>
		</Screen>

		<template v-for="(index, i) of train">
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

		<Screen :title="'Testing Round 2'">
			When you are ready, click the button to start your test session.
			<button @click="prepareTest();$magpie.nextScreen();">
				Next
			</button>
		</Screen>

		<template v-for="(index, i) of test">
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
		<Screen :title="'Training Round 3a'">
			When you are ready, click the button to start your final round of training.
			<button @click="shuffleSeen();$magpie.nextScreen();">
				Next
			</button>
		</Screen>

		<template v-for="(index, i) of train">
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

		<Screen :title="'Training Round 3b'">
			When you are ready, click the button to start your next training session.
			<button @click="shuffleSeen();$magpie.nextScreen();">
				Next
			</button>
		</Screen>

		<template v-for="(index, i) of train">
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
		<Screen :title="'Final Testing'">
			You are almost there!
			<br>
			<br>
			When you are ready, click the button to start your final test for the language.
			<button @click="prepareFinal();$magpie.nextScreen();">
				Next
			</button>
		</Screen>

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
		let train = [];
		let test = [];
		let final = [];

		return {
			pictures,
			pics,
			vocab,
			seen,
			unseen,
			train,
			test,
			final,
		};
	},

	created: function(){
		this.getPictures();
		this.generateVocab();
		this.splitSeenUnseen();
	},

	methods: {

		getPictures:
		 function(){
			this.pics = pictures;
		 },

		generateVocab:
		 function(){
			// generate new vocab for each chain
			this.vocab = [];
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
			if (indices.length < 5){ // too few training instances
				// reset vocab
				this.generateVocab()
				indices = [...Array(vocab_size).keys()] // [0, 1, ..., 27]
				console.log(this.vocab)
				console.log(indices)
			}
			else if (indices.length < num_seen){  // if too many ambiguous words
				num_seen = indices.length
				num_train = Math.ceil(num_seen / 2)
				num_test = Math.ceil((vocab_size - num_seen) / 2)
			}
			let sampled_unique_vocab_indices = this.sample_n(num_seen, indices)
			this.seen = _at(indices, sampled_unique_vocab_indices)
			this.unseen = _shuffle(_difference(Array.from(this.pictures, (_, idx) => idx), this.seen))
		 },

		splitSeenUnseen:
		 function(){
			this.seen = this.sample_n(num_seen, this.vocab)
			let indices = Array.from(this.pictures, (_, idx) => idx)
			this.unseen = _shuffle(_difference(indices, this.seen))
		 },

		shuffleSeen:
		 function(){
		 	this.train = _shuffle(this.seen)
		 },

		prepareTest:
		 function(){
		 	this.test = _shuffle(_slice(_shuffle(this.unseen), num_test).concat(_slice(_shuffle(this.seen), num_train)))
		 },

		prepareFinal:
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
