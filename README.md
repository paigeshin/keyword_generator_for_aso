# keyword_generator_for_aso

```jsx
function extractKeywords(titlesArray, exstingKeywordsArray, keywordsArray) {
  let existingKeywords = [...exstingKeywordsArray];
  let resultKeywords = [];
  let charCount = 0;

  // extract all words from titles and subtitles
  titlesArray.forEach((item) => {
    existingKeywords = existingKeywords.concat(
      ...item.title
        .toLowerCase()
        .replace(/[^a-zA-Z0-9\s]/g, "")
        .split(" "),
      ...item.subtitle
        .toLowerCase()
        .replace(/[^a-zA-Z0-9\s]/g, "")
        .split(" ")
    );
  });

  let keywords = shuffled(keywordsArray);
  keywords = redundantPluralsRemoved(keywords);

  keywords.forEach((keyword) => {
    const currentKeywords = keyword
      .toLowerCase()
      .replace(/[^a-zA-Z0-9\s]/g, "")
      .split(" ");

    let totalWordCount = currentKeywords.reduce(
      (acc, word) => acc + word.length,
      0
    );

    // Check if any word in currentKeywords is in extractedTitles or resultKeywords
    const anyWordExists = currentKeywords.some(
      (word) => existingKeywords.includes(word) || resultKeywords.includes(word)
    );

    if (anyWordExists) {
      return;
    }

    // If totalWordCount + charCount exceeds 100 or any word exists, skip the currentKeywords
    if (charCount + totalWordCount > 100) {
      return;
    }

    resultKeywords.push(...currentKeywords);
    charCount += totalWordCount;
  });

  return resultKeywords;
}

function shuffled(array) {
  for (let i = array.length - 1; i > 0; i--) {
    // Generate random index
    const j = Math.floor(Math.random() * (i + 1));

    // Swap elements at index i and j
    const temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }
  return array;
}

function redundantPluralsRemoved(arr) {
  const result = [];
  const seenWords = new Set();

  // Sort so singular comes before plural
  arr.sort();

  for (const word of arr) {
    if (seenWords.has(word)) continue; // If this word was processed, skip

    if (arr.includes(word + "s")) {
      result.push(word);
      seenWords.add(word);
      seenWords.add(word + "s"); // Mark both singular and plural as seen
    } else {
      result.push(word);
      seenWords.add(word);
    }
  }

  return result;
}

function duplicatesRemoved(arr) {
  return [...new Set(arr)];
}

const inputTitles = [
  {
    title: "Bulking with Ideal Protein, Super Power",
    subtitle: "Gain weight, muscle, meals",
  },
  {
    title: "Arise Bodybuilding meal plan",
    subtitle: "Body building app, bulq, plate",
  },
];

const existingKeywords = [
  "foodnoms",
  "bulk",
  "myprotein",
  "tracking",
  "well",
  "menu",
  "planner",
  "my",
  "fitness",
  "pal",
  "myfitnesspal",
  "calories",
  "diary",
  "macros",
  "clean",
  "simple",
  "eats",
];

const inputKeywords = [
  "Bulking with Ideal Protein, Super Power",
  "planning power hahaha kk",
  "workout",
  "healthy",
  "fitnesspal",
  "fit",
  "fit",
  "fit",
  "fit",
  "fits",
  "fitness",
  "fitnesses",
  "cycling",
  "para",
  "fire",
  "prep",
  "foodnoms",
  "bulk",
  "myprotein",
  "tracking",
  "well",
  "menu",
  "planner",
  "my",
  "fitness",
  "pal",
  "myfitnesspal",
  "calories",
  "diary",
  "macros",
  "clean",
  "simple",
  "eats",
  "intake",
  "strength",
];

const result = extractKeywords(
  inputTitles,
  existingKeywords,
  shuffled(inputKeywords)
);
console.log(result);
let totalWordCount = result.reduce((acc, word) => acc + word.length, 0);
console.log(totalWordCount);

```
