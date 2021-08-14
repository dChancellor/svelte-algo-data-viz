<script>
  let hash = {};
  let ans = [];
  let pairedNumber = 0;
  let currentMicroStep = 0;
  let pointer = 0;
  const input = { array: [3, 5, -4, 8, 11, 1, -1, 6], targetSum: 10 };

  const checkIfAnswerExists = () => (ans.length > 0 ? true : false);

  function checkIfTraversalComplete() {
    if (pointer === input.array.length) return true;
  }

  const definePairedNumber = () => {
    pairedNumber = input.targetSum - input.array[pointer];
  };

  const checkIfPairedNumberExists = () => {
    if (hash[pairedNumber]) ans = [input.array[pointer], pairedNumber];
  };

  const addCurrentNumberToHash = () => {
    hash = { ...(hash[input.array[pointer]] = true), ...hash };
    pointer += 1;
  };

  const handleMicroStep = () => {
    currentMicroStep = currentMicroStep === microSteps.length - 1 ? 0 : (currentMicroStep += 1);
  };
  $: microSteps[currentMicroStep]();

  const handleFullStep = () => {
    for (let i = currentMicroStep; i < microSteps.length; i += 1) {
      microSteps[i]();
    }
  };
  let microSteps = [
    checkIfAnswerExists,
    checkIfTraversalComplete,
    definePairedNumber,
    checkIfPairedNumberExists,
    addCurrentNumberToHash,
  ];

  $: console.log(ans);
</script>

<main>
  <button on:click={handleMicroStep}>Micro Step </button>
  <button on:click={handleFullStep}>Full Step </button>
  <p>{`for(let i = 0; i<array.length; i+=1){`}</p>
  <p>{`\xa0\xa0\xa0\xa0\xa0 let pairedNumber = targetSum-array[i]`}</p>
  <p>{`\xa0\xa0\xa0\xa0\xa0 if(hash[pairedNumber]) ans = [array[i], pairedNumber]`}</p>
  <p>{`\xa0\xa0\xa0\xa0\xa0 hash[array[i]] = true;`}</p>
  <p>{` }`}</p>
  <p>{`return ans;`}</p>
  {#each input.array as item}
    <p>{item}</p>
  {/each}
  {#each Object.entries(hash) as [key, value]}
    <p>{key}</p>
  {/each}
  {#if ans.length > 0}
    <h2>{`${ans[0]}, ${ans[1]}`}</h2>
  {/if}
</main>

<style>
</style>
