# iterated-minimal

test experiment link on Netlify: https://test-experiment-blackfooted.netlify.app

## To-Do
- implement getPreviousResponse
  - retrieve data from server (chain, generation)
  - retrieve data from previous generation (item, input)
  - do filtering (make sure this.seen is unique by moving ambiguous items' indices to this.unseen)
  - generate all data needed for the experiment again (replace this.vocab, refer to pic by 'item' instead of index)
