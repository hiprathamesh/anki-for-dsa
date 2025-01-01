<script>
  // Common complexity options
  const complexityOptions = [
    "O(1)",
    "O(log n)",
    "O(n)",
    "O(n log n)",
    "O(n²)",
    "O(n³)",
    "O(2ⁿ)",
    "O(n!)",
    "Custom",
  ];

  // Question types
  const questionTypes = [
    "Array",
    "String",
    "Linked List",
    "Binary Search",
    "Binary Tree",
    "Graph",
    "Dynamic Programming",
    "Backtracking",
    "Stack",
    "Queue",
    "Heap",
  ];

  // Difficulty levels
  const difficultyLevels = ["Easy", "Medium", "Hard"];

  // Form data structure
  let formData = {
    questionName: "",
    questionType: questionTypes[0],
    difficulty: difficultyLevels[0],
    approaches: {
      brute: [
        {
          approach: "",
          tc: complexityOptions[0],
          sc: complexityOptions[0],
          customTc: "",
          customSc: "",
        },
      ],
      better: [
        {
          approach: "",
          tc: complexityOptions[0],
          sc: complexityOptions[0],
          customTc: "",
          customSc: "",
        },
      ],
      optimal: [
        {
          approach: "",
          tc: complexityOptions[0],
          sc: complexityOptions[0],
          customTc: "",
          customSc: "",
        },
      ],
    },
  };

  // Add new approach section
  function addApproach(type) {
    formData.approaches[type] = [
      ...formData.approaches[type],
      {
        approach: "",
        tc: complexityOptions[0],
        sc: complexityOptions[0],
        customTc: "",
        customSc: "",
      },
    ];
  }

  // Remove approach section
  function removeApproach(type, index) {
    formData.approaches[type] = formData.approaches[type].filter(
      (_, i) => i !== index
    );
  }

  // Get actual complexity (either selected or custom)
  function getComplexity(selected, custom) {
    return selected === "Custom" ? custom : selected;
  }

  // Generate formatted text
  function getFormattedTitle() {
    return `${formData.questionName} (${formData.questionType}, ${formData.difficulty})`;
  }

  function getFormattedApproaches() {
    let result = "";

    // Format brute approaches
    formData.approaches.brute.forEach((approach, index) => {
      if (approach.approach) {
        result += `Brute${formData.approaches.brute.length > 1 ? index + 1 : ""}: ${approach.approach}\n`;
        result += `TC: ${getComplexity(approach.tc, approach.customTc)}  |  SC: ${getComplexity(approach.sc, approach.customSc)}\n\n`;
      }
    });

    // Format better approaches
    formData.approaches.better.forEach((approach, index) => {
      if (approach.approach) {
        result += `Better${formData.approaches.better.length > 1 ? index + 1 : ""}: ${approach.approach}\n`;
        result += `TC: ${getComplexity(approach.tc, approach.customTc)}  |  SC: ${getComplexity(approach.sc, approach.customSc)}\n\n`;
      }
    });

    // Format optimal approaches
    formData.approaches.optimal.forEach((approach, index) => {
      if (approach.approach) {
        result += `Optimal${formData.approaches.optimal.length > 1 ? index + 1 : ""}: ${approach.approach}\n`;
        result += `TC: ${getComplexity(approach.tc, approach.customTc)}  |  SC: ${getComplexity(approach.sc, approach.customSc)}\n\n`;
      }
    });

    return result.trim();
  }

  // Copy text to clipboard
  async function copyToClipboard(text) {
    try {
      await navigator.clipboard.writeText(text);
    } catch (err) {
      console.error("Failed to copy text: ", err);
    }
  }

  // Copy all text
  function copyAll() {
    const title = getFormattedTitle();
    const approaches = getFormattedApproaches();
    const allText = `${title}\n\n${approaches}`;
    copyToClipboard(allText);
  }
</script>

<main class="container">
  <h1>Anki Cards Generator</h1>

  <div class="content-wrapper">
    <div class="q-section">
      <div class="form-group">
        <label for="questionName">Question Name:</label>
        <style>
          #questionName {
            width: calc(100% - 16px); /* Accounts for padding */
          }
        </style>
        <input
          id="questionName"
          type="text"
          bind:value={formData.questionName}
          required
        />
      </div>

      <div class="form-group">
        <label for="questionType">Question Type:</label>
        <select id="questionType" bind:value={formData.questionType}>
          {#each questionTypes as type}
            <option value={type}>{type}</option>
          {/each}
        </select>
      </div>

      <div class="form-group">
        <label for="difficulty">Difficulty:</label>
        <select id="difficulty" bind:value={formData.difficulty}>
          {#each difficultyLevels as level}
            <option value={level}>{level}</option>
          {/each}
        </select>
      </div>
    </div>
    <div class="form-section">
      <!-- Brute Force Approaches -->
      <div class="approach-section">
        <h2>
          Brute Force Approaches
          <button class="add-btn" on:click={() => addApproach("brute")}
            >+</button
          >
        </h2>
        {#each formData.approaches.brute as bruteApproach, index}
          <div class="approach-container">
            <style>
              #approachText {
                width: calc(100% - 16px); /* Accounts for padding */
              }
            </style>
            <textarea
              id="approachText"
              placeholder="Brute force approach description"
              bind:value={bruteApproach.approach}
            ></textarea>
            <div class="complexity-group">
              <div class="complexity-input">
                <select bind:value={bruteApproach.tc}>
                  {#each complexityOptions as option}
                    <option value={option}>{option}</option>
                  {/each}
                </select>
                {#if bruteApproach.tc === "Custom"}
                  <input
                    type="text"
                    placeholder="Enter custom TC"
                    bind:value={bruteApproach.customTc}
                  />
                {/if}
              </div>
              <div class="complexity-input">
                <select bind:value={bruteApproach.sc}>
                  {#each complexityOptions as option}
                    <option value={option}>{option}</option>
                  {/each}
                </select>
                {#if bruteApproach.sc === "Custom"}
                  <input
                    type="text"
                    placeholder="Enter custom SC"
                    bind:value={bruteApproach.customSc}
                  />
                {/if}
              </div>
              {#if index > 0}
                <button
                  class="remove-btn"
                  on:click={() => removeApproach("brute", index)}>×</button
                >
              {/if}
            </div>
          </div>
        {/each}
      </div>

      <!-- Better Approaches -->
      <div class="approach-section">
        <h2>
          Better Approaches
          <button class="add-btn" on:click={() => addApproach("better")}
            >+</button
          >
        </h2>
        {#each formData.approaches.better as betterApproach, index}
          <div class="approach-container">
            <style>
              #approachText {
                width: calc(100% - 16px); /* Accounts for padding */
              }
            </style>
            <textarea
              id="approachText"
              placeholder="Better approach description"
              bind:value={betterApproach.approach}
            ></textarea>
            <div class="complexity-group">
              <div class="complexity-input">
                <select bind:value={betterApproach.tc}>
                  {#each complexityOptions as option}
                    <option value={option}>{option}</option>
                  {/each}
                </select>
                {#if betterApproach.tc === "Custom"}
                  <input
                    type="text"
                    placeholder="Enter custom TC"
                    bind:value={betterApproach.customTc}
                  />
                {/if}
              </div>
              <div class="complexity-input">
                <select bind:value={betterApproach.sc}>
                  {#each complexityOptions as option}
                    <option value={option}>{option}</option>
                  {/each}
                </select>
                {#if betterApproach.sc === "Custom"}
                  <input
                    type="text"
                    placeholder="Enter custom SC"
                    bind:value={betterApproach.customSc}
                  />
                {/if}
              </div>
              {#if index > 0}
                <button
                  class="remove-btn"
                  on:click={() => removeApproach("better", index)}>×</button
                >
              {/if}
            </div>
          </div>
        {/each}
      </div>

      <!-- Optimal Approaches -->
      <div class="approach-section">
        <h2>
          Optimal Approaches
          <button class="add-btn" on:click={() => addApproach("optimal")}
            >+</button
          >
        </h2>
        {#each formData.approaches.optimal as optimalApproach, index}
          <div class="approach-container">
            <style>
              #approachText {
                width: calc(100% - 16px); /* Accounts for padding */
              }
            </style>
            <textarea
              id="approachText"
              placeholder="Optimal approach description"
              bind:value={optimalApproach.approach}
              required
            ></textarea>
            <div class="complexity-group">
              <div class="complexity-input">
                <select bind:value={optimalApproach.tc}>
                  {#each complexityOptions as option}
                    <option value={option}>{option}</option>
                  {/each}
                </select>
                {#if optimalApproach.tc === "Custom"}
                  <input
                    type="text"
                    placeholder="Enter custom TC"
                    bind:value={optimalApproach.customTc}
                  />
                {/if}
              </div>
              <div class="complexity-input">
                <select bind:value={optimalApproach.sc}>
                  {#each complexityOptions as option}
                    <option value={option}>{option}</option>
                  {/each}
                </select>
                {#if optimalApproach.sc === "Custom"}
                  <input
                    type="text"
                    placeholder="Enter custom SC"
                    bind:value={optimalApproach.customSc}
                  />
                {/if}
              </div>
              {#if index > 0}
                <button
                  class="remove-btn"
                  on:click={() => removeApproach("optimal", index)}>×</button
                >
              {/if}
            </div>
          </div>
        {/each}
      </div>
    </div>

    <div class="output-section">
      <div class="output-container">
        <h3>Title</h3>
        <div class="output-box">
          <p>{getFormattedTitle()}</p>
          <button on:click={() => copyToClipboard(getFormattedTitle())}>
            Copy
          </button>
        </div>
      </div>

      <div class="output-container">
        <h3>Approaches</h3>
        <div class="output-box">
          <pre>{getFormattedApproaches()}</pre>
          <button on:click={() => copyToClipboard(getFormattedApproaches())}>
            Copy
          </button>
        </div>
      </div>
      <button class="copy-all-btn" on:click={copyAll}> Copy All </button>
      <span class="credits">royalp © 2025</span>
    </div>
  </div>
</main>

<style>
  .credits {
    display: block;
    font-size: 14px;
    text-align: center;
    margin-top: 30px;
    color: #666;
  }

  .q-section {
    flex: 0 0 45%;
    max-width:600px;
  }

  .container {
    max-width: 1400px;
    margin: 0 auto;
    padding: 20px;
    font-family: Arial, sans-serif;
  }

  .content-wrapper {
    display: flex;
    gap: 80px;
    align-items: flex-start;
    justify-content: center;
  }

  .form-section {
    flex: 0 0 55%;
    max-width: 800px;
  }

  .output-section {
    flex: 0 0 35%;
    position: sticky;
    top: 20px;
    max-height: calc(100vh - 100px);
    overflow-y: auto;
    padding-right: 10px;
  }

  h1 {
    text-align: center;
    color: #333;
  }

  .form-section {
    margin-bottom: 30px;
  }

  .form-group {
    margin-bottom: 15px;
  }

  .form-group label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
  }

  input,
  select,
  textarea {
    width: 100%;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    margin-bottom: 10px;
  }

  textarea {
    height: 80px;
    resize: vertical;
  }

  .approach-section {
    margin-bottom: 20px;
    padding: 15px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }

  .approach-section h2 {
    margin-top: 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .complexity-group {
    display: flex;
    gap: 10px;
    align-items: flex-start;
  }

  .complexity-input {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 5px;
  }

  .complexity-input select {
    width: 100%;
  }

  .complexity-input input {
    width: 100%;
  }

  .add-btn,
  .remove-btn {
    padding: 5px 10px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .add-btn {
    background-color: #4caf50;
    color: white;
  }

  .remove-btn {
    background-color: #f44336;
    color: white;
    flex-shrink: 0;
  }

  .output-section {
    margin-top: 0;
  }

  .copy-all-btn {
    width: 100%;
    padding: 12px;
    background-color: #2196f3;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
    font-weight: bold;
    margin-top: 20px;
    transition: background-color 0.2s;
  }

  .copy-all-btn:hover {
    background-color: #1976d2;
  }

  .output-container {
    margin-bottom: 20px;
  }

  .output-box {
    position: relative;
    background-color: #303030;
    padding: 15px;
    border-radius: 4px;
    margin-bottom: 20px;
    min-width: 200px;
    min-height: 50px;
  }

  .output-box pre {
    white-space: pre-wrap;
    margin: 0;
    font-family: monospace;
  }

  .output-box button {
    position: absolute;
    top: 10px;
    right: 10px;
    padding: 5px 10px;
    background-color: #2196f3;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .output-box button:hover {
    background-color: #1976d2;
  }
</style>
