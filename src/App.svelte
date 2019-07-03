<section>
  <textarea bind:this={inputBox} on:input={hangeulize} placeholder="Romaja" rows="1"></textarea>
</section>
<section>
  <textarea bind:this={outputBox} placeholder="한글" rows="1"></textarea>
</section>

<p>Romaja to 한글 converter by <a href="https://anthony.som.codes">Anthony Som</a>.</p>

<script>
  export let inputBox;
  export let outputBox;

  import HANGEUL_DICTIONARY from './dict.js'

  export function hangeulize(event) {
    let romaja = inputBox.value

    let hangeul = []
    let currentNode = HANGEUL_DICTIONARY

    for (let i = romaja.length - 1; i >= 0; --i) {
        let r = romaja[i].toUpperCase()
        let next = currentNode[r]
        if (!next && currentNode["$"]) {
            hangeul.push(currentNode["$"])
            next = HANGEUL_DICTIONARY[r]
        }
        if (!next) {
            if (romaja[i] != "-") hangeul.push(romaja[i])
            next = HANGEUL_DICTIONARY
        }
        currentNode = next
    }
    if (currentNode["$"]) hangeul.push(currentNode["$"])

    outputBox.value = hangeul.reverse().join("")
  }
</script>
