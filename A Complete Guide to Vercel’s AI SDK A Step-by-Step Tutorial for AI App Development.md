A Complete Guide to Vercel’s AI SDK: A Step-by-Step Tutorial for [[AI]] App Development
Vercel’s AI SDK has emerged as a transformative toolkit for developers working with [[large language models (LLMs]]) and AI-powered applications. This article provides a comprehensive talk that walks you through every step—from installation and basic text generation to advanced topics like streaming output, structured data extraction, tool calling, and building intelligent agents. Whether you’re new to the world of AI or an experienced developer, this guide aims to explain every detail in a clear, step‐by‐step manner.

---

## Table of Contents

1. Introduction
2. Why Use Vercel’s AI SDK?
3. Installation and Setup
4. Understanding the Core Components
    - Core SDK
    - AI SDK UI
    - AI SDK RSC
5. Basic Text Generation
6. Streaming Text Output
7. Working with Structured Output
8. integrating Local AI Models
9. Tool Calling: Letting AI Interact with the World
10. Building AI Agents with Multi-Step Reasoning
11. Extracting Data from PDFs
12. Creating Embeddings and Vector Databases
13. Conclusion and Next Steps

---

## Introduction

In today’s rapidly evolving AI landscape, developers are continuously looking for ways to simplify interactions with powerful language models. Vercel’s AI SDK is designed to be the “standard library” for AI development in JavaScript environments. With its unified interface, the SDK streamlines the process of integrating different AI models, handling streaming responses, structuring outputs, and even enabling communication with external tools.

The SDK eliminates the common challenges associated with switching providers—for example, moving from OpenAI to Anthropic—by abstracting the differences in API designs. In this guide, we’ll break down each aspect of the SDK so you can understand not only how to use it but also why it is so valuable for modern AI development. 

---

## Why Use Vercel’s AI SDK?

Imagine you have built an AI application that uses OpenAI’s API to generate text responses. Now, if you decide to test a different provider such as Anthropic, you might need to write extra “glue” code to account for differences in their API structure. This situation is common in AI app development and can lead to code duplication, maintenance issues, and a slower pace of innovation.

**Key benefits include:**

- **Unified API Across Providers:**  
    The SDK provides a consistent interface to interact with various LLM providers. This means that you can switch models with minimal code changes.
    
- **Cross-Platform Compatibility:**  
    Whether you’re running your code on Node.js, Deno, or even in a browser-based environment, the AI SDK works wherever JavaScript runs.
    
- **Support for Advanced Features:**  
    Out-of-the-box features such as streaming responses, structured outputs, tool calling, and conversation history management are built into the SDK.
    
- **Free and Open-Source:**  
    Vercel offers the SDK as open-source software, reducing barriers to entry and promoting community contributions.
    

By consolidating these functionalities into one tool, the AI SDK helps you build robust AI applications faster and more efficiently.

---

## Installation and Setup

Before diving into the code, you need to install the necessary packages. Vercel’s AI SDK is split into several packages to accommodate different functionalities.

### Installing the Core Package

The core package contains the essential functions for interacting with LLMs. Install it via npm with:

```sh
npm install ai
```

### Installing Provider-Specific Packages

Depending on your preferred LLM provider, install the relevant integration package:

- **For OpenAI:**
    
    ```sh
    npm install @vercel/ai-sdk-openai
    ```
    
- **For Anthropic:**
    
    ```sh
    npm install @vercel/ai-sdk-anthropic
    ```
    

After installation, you’re ready to start building your AI-powered application!

---

## Understanding the Core Components

Vercel’s AI SDK is divided into three main components, each designed to handle different aspects of AI integration. Understanding these components is critical for using the SDK effectively.

### Core SDK

**Purpose:**  
The Core SDK is the backbone for backend integration. It provides a provider-agnostic API for generating text, handling streaming responses, and more.

**Key Functions:**

- **generateText:** For generating a complete text response from an LLM.
- **streamText:** For receiving text output incrementally, useful for real-time applications.
- **generateObject:** For receiving structured outputs, typically used when expecting data in a predefined format.

This core component allows you to build complex AI interactions without worrying about the underlying API differences between providers. (cite_sdk_vercel_docs)

### AI SDK UI

**Purpose:**  
The UI component offers framework-agnostic hooks and components that help integrate AI functionalities into your frontend applications.

**Benefits:**

- **Ease of Integration:** Use the provided hooks in React, Next.js, or other frameworks to quickly integrate chat interfaces and generative UIs.
- **Seamless User Experience:** Manage conversation histories and real-time updates on the client side.

### AI SDK RSC (React Server Components)

**Purpose:**  
This experimental module is designed to support React Server Components for generative UIs that require server-rendered, streaming updates.

**Considerations:**

- **Streaming UI:** Use this component to stream dynamic content directly from the server to the client.
- **Experimental:** Although powerful, it is recommended to use the more mature UI components for production.

For this article, our focus remains on the Core SDK, which lays the foundation for all subsequent functionalities.

---

## Basic Text Generation

One of the most common tasks in AI applications is generating text responses from a prompt. Vercel’s AI SDK simplifies this process with the `generateText` function.

### Step-by-Step Explanation

14. **Import Required Functions:** Begin by importing the `generateText` function from the core package and the provider-specific integration (e.g., OpenAI):

    ```javascript
    import { generateText } from 'ai';
    import { openai } from '@vercel/ai-sdk-openai';
    ```

15. **Select Your Model:** Use the provider integration to create a model instance. Here, we’re using OpenAI’s GPT-4 model:

    ```javascript
    const model = openai({ model: 'gpt-4' });
    ```

16. **Create an Asynchronous Function:** Define a function, for example, `answerQuestion`, that accepts a prompt as input. The function awaits the text generation:

    ```javascript
    async function answerQuestion(prompt) {
      const result = await generateText({ model, prompt });
      console.log(result.text);
    }
    ```

17. **Call the Function with a Prompt:** Finally, invoke the function with a question:

    ```javascript
    answerQuestion("What is the chemical formula for water?");
    ```

    When executed, this code calls the OpenAI API via the unified SDK interface and logs “H2O” as the output.


This simple example highlights how few lines of code are needed to interact with a powerful LLM using the AI SDK.

---

## Streaming Text Output

For applications requiring real-time feedback—such as chatbots or live notifications—streaming text output is crucial. The SDK provides the `streamText` function for this purpose.

### Detailed Walkthrough

18. **Import the Streaming Function:**

    ```javascript
    import { streamText } from 'ai';
    ```

19. **Define the Streaming Function:** Create a function, such as `streamResponse`, that accepts a prompt. Notice that this function is asynchronous because it processes a stream of data:

    ```javascript
    async function streamResponse(prompt) {
      const stream = await streamText({ model, prompt });
      for await (const chunk of stream) {
        process.stdout.write(chunk);
      }
    }
    ```

20. **Explanation:**
    
    - **Stream Initialization:**  
        The `streamText` function sends a prompt to the model and returns an asynchronous iterator that provides chunks of text as they are generated.
        
    - **For-Await Loop:**  
        The `for await...of` loop waits for each text chunk (or token) and writes it to the standard output. This simulates a live, streaming experience.
        
21. **Execute the Streaming Function:**

    ```javascript
    streamResponse("Tell me a story about a brave knight.");
    ```


This approach is especially useful when you want users to see the answer as it’s being formulated, rather than waiting for the complete text to be generated. Real-time streaming improves interactivity and responsiveness in many AI applications.

---

## Working with Structured Outputs

Often, AI applications need to extract data in a structured format rather than plain text. This can be especially helpful when parsing recipes, invoices, or any information where data types matter. The SDK’s `generateObject` function, combined with a schema defined using libraries like [Zod](https://github.com/colinhacks/zod) (a TypeScript-first schema declaration and validation library), allows you to achieve this.

### Step-by-Step Example: Extracting a Recipe

22. **Import the Necessary Modules:**

    ```javascript
    import { generateObject } from 'ai';
    import { z } from 'zod';
    ```

23. **Define a Schema:** Create a Zod schema that defines the expected structure of the output. In this example, the recipe contains a name, a list of ingredients (each with a name and quantity), and an array of steps:

    ```javascript
    const RecipeSchema = z.object({
      name: z.string().describe("Recipe name"),
      ingredients: z.array(z.object({
        name: z.string(),
        quantity: z.string()
      })).describe("List of ingredients"),
      steps: z.array(z.string()).describe("Cooking steps")
    });
    ```

    - **Describe Function:**  
        Using `.describe()` helps the language model understand what each property represents—a form of prompt engineering to guide the LLM toward generating the correct structure.
24. **Create the Function to Generate the Object:**

    ```javascript
    async function getRecipe(dish) {
      const result = await generateObject({ 
        model, 
        prompt: `Give me a recipe for ${dish}`, 
        schema: RecipeSchema 
      });
      console.log(result.object);
    }
    ```

25. **Call the Function:**

    ```javascript
    getRecipe("Pasta Carbonara");
    ```

26. **What Happens Behind the Scenes:**
    
    - The SDK sends the prompt along with the Zod schema to the model.
    - The model generates a response that conforms to the defined schema.
    - The generated object is then validated against the schema, ensuring type safety and predictable structure.

This structured approach not only makes it easier to work with the output in your application but also leverages TypeScript’s auto-completion features for a smoother developer experience.

---

## Integrating Local AI Models

While many developers use cloud-based models like OpenAI, there’s a growing interest in running models locally for reasons such as latency, cost, or data privacy. Vercel’s AI SDK supports integration with local models via OpenAI-compatible APIs.

### How to Set Up a Local Model

27. **Install a Local Model Server:** Tools such as [LM Studio](https://lmstudio.ai/) can be used to host an AI model locally. Once installed, the model server typically runs on a local URL (e.g., `http://localhost:1234`).

28. **Import the Compatibility Function:**

    ```javascript
    import { createOpenAICompatible } from '@vercel/ai-sdk-openai';
    ```

29. **Create the Local Model Instance:**

    ```javascript

    const localModel = createOpenAICompatible({ baseUrl: 'http://localhost:1234' });
    ```

31. **Use the Local Model as You Would Any Other:** Once configured, you can use `localModel` in functions like `generateText`, `streamText`, or `generateObject` in the same way you’d use a cloud-based model.


Integrating a local model helps maintain consistent development practices while giving you the flexibility to run models in your own environment.

---

## Tool Calling: Letting AI Interact with the World

A unique feature of Vercel’s AI SDK is the ability to have the language model call external tools. This feature allows the model not only to generate text but also to trigger functions or interact with external systems—effectively bridging the gap between AI and real-world operations.

### Step-by-Step Guide for Tool Calling

32. **Import the Tool Function and Zod Library:**

    ```javascript
    import { tool } from 'ai';
    import { z } from 'zod';
    ```

33. **Define a Tool:** For this example, we’ll create a simple tool that logs messages to the console. Define the tool with a name, a Zod schema for the expected parameters, and an `execute` function that performs the action:

    ```javascript
    const logToConsoleTool = tool({
      name: 'logToConsole',
      schema: z.object({ message: z.string() }),
      execute: async ({ message }) => {
        console.log(message);
      },
    });
    ```

34. **Integrate the Tool into a Function:** You can then create a function that uses this tool. For example, when the model receives a prompt, it might decide to call the `logToConsole` tool to output “Hello, world!” to the console.

    ```javascript
    async function callTool() {
      await logToConsoleTool.execute({ message: "Hello, world!" });
    }
    
    callTool();
    ```

35. **What’s Happening Under the Hood:**
    
    - The SDK sends the prompt along with a list of available tools.
    - The model “decides” (based on its internal logic) which tool to call and with what parameters.
    - The tool’s `execute` function is then invoked, and its output can be fed back into further model interactions if needed.

This mechanism is extremely powerful because it lets you build systems where the model can interact with external APIs, databases, or any arbitrary function.

---

## Building AI Agents with Multi-Step Reasoning

Modern AI applications often require more than a single call to an LLM—they need an agent that can take multiple steps, react to intermediate outputs, and iterate on a solution. Vercel’s AI SDK makes this possible with agentic loops.

### Step-by-Step Example: A Weather Agent

36. **Set Up the Environment:** Begin by importing the `streamText` function, which will allow you to handle multi-step interactions:

    ```javascript
    import { streamText } from 'ai';
    ```

37. **Create the Weather Function:** Define a function `getWeather` that accepts a city name and uses the `streamText` function. Notice that we include a `tools` property to simulate external calls—in this case, a simple function that returns a weather string.

    ```javascript
    async function getWeather(city) {
      const stream = await streamText({
        model,
        prompt: `Get the weather in ${city}`,
        tools: { 
          getWeather: async (city) => `25°C and sunny in ${city}` 
        },
        maxSteps: 2  // Limits the number of interaction iterations
      });
      for await (const chunk of stream) {
        process.stdout.write(chunk);
      }
    }
    
    getWeather("London");
    ```

38. **Understanding the Process:**

    - **Multi-Step Interaction:**  
        The `maxSteps` property limits the number of iterations. In our example, the model can take two steps: it might first call the tool to get weather information and then use that information to generate a final response.
    - **Tool Feedback Loop:**  
        The tool call result is fed back into the model, creating a loop where the LLM can refine its output or take additional actions.
    - **Safety Mechanism:**  
        The `maxSteps` property also acts as a safeguard to prevent endless loops, ensuring that the agent stops once its task is completed.

Building AI agents that take multiple steps can help create more reliable and context-aware systems—minimizing hallucinations and improving output relevance. 

---

## Extracting Data from PDFs

One of the more challenging tasks in AI applications is extracting structured data from unstructured documents like PDFs. With Vercel’s AI SDK, you can pass files (e.g., PDFs) to an LLM and have it extract key pieces of information based on a predefined schema.

### Detailed Walkthrough

39. **Import Modules and Read the File:**

    ```javascript
    import fs from 'fs';
    import { generateObject } from 'ai';
    import { z } from 'zod';
    ```

    Use Node.js’s `fs` module to read the file into memory as a binary (Uint8Array).

40. **Define a Schema for the PDF Data:** Suppose you want to extract details from an invoice. Define a Zod schema that includes fields such as total amount, currency, and invoice number.

    ```javascript
    const InvoiceSchema = z.object({
      total: z.string(),
      currency: z.string(),
      invoiceNumber: z.string(),
    });
    ```

41. **Create the Data Extraction Function:**

    ```javascript
    async function extractInvoiceData(pdfPath) {
      const pdfData = fs.readFileSync(pdfPath);
      const result = await generateObject({
        model,
        prompt: "Extract invoice details",
        schema: InvoiceSchema,
        messages: [{
          type: 'file', 
          data: pdfData, 
          mimeType: 'application/pdf'
        }],
      });
      console.log(result.object);
    }
    
    extractInvoiceData("./invoice.pdf");
    ```

42. **Explanation:**
    
    - **File Loading:**  
        The PDF file is loaded into memory as a raw binary (Uint8Array). This allows the LLM to “read” the file content.
    - **Schema Validation:**  
        By providing a schema, you ensure that the output conforms to a specific structure. This means that once the data is extracted, you can easily manipulate or store it in your database.
    - **Versatility:**  
        Although this example focuses on invoices, you can extend this approach to any document where structured extraction is needed.

This approach simplifies a historically challenging problem—converting unstructured PDFs into structured, queryable data—by harnessing the power of LLMs with clear, developer-friendly APIs.

---

## Creating Embeddings and Vector Databases

Embeddings are numerical representations of text, images, or other data in high-dimensional space. They enable powerful semantic similarity comparisons, which are essential for search, clustering, and recommendation systems.

### Step-by-Step Explanation

43. **Import the Embedding Functions:**

    ```javascript
    import { embedMany, embed } from 'ai';
    ```

44. **Generate Embeddings for a List of Words:**

    ```javascript
    async function createEmbeddings() {
      const words = ["dog", "cat", "car", "bike"];
      const embeddings = await embedMany({ model, values: words });
      // Each element in 'embeddings' is a vector representation
      console.log(embeddings);
    }
    
    createEmbeddings();
    ```

45. **Build a Simple Vector Database:** In a production system, you might store these embeddings in a database like PostgreSQL with PGVector. For a simple demo, use an array to store pairs of words and their corresponding embeddings.

    ```javascript
    async function buildVectorDatabase() {
      const words = ["dog", "cat", "car", "bike"];
      const embeddings = await embedMany({ model, values: words });
      const vectorDatabase = words.map((word, index) => ({
        value: word,
        embedding: embeddings[index]
      }));
      console.log(vectorDatabase);
    }
    
    buildVectorDatabase();
    ```

46. **Performing a Similarity Search:** To find the word most similar to a given search term, first embed the search term, then use the cosine similarity function provided by the SDK to compare vectors.

    ```javascript
    async function searchSimilarWord(searchTerm) {
      const searchEmbedding = await embed({ model, value: searchTerm });
      // Assume vectorDatabase is already built
      const vectorDatabase = await buildVectorDatabase();
      const similarityResults = vectorDatabase.map(item => {
        return {
          word: item.value,
          similarity: cosineSimilarity(item.embedding, searchEmbedding)
        };
      });
      similarityResults.sort((a, b) => b.similarity - a.similarity);
      console.log("Most similar word:", similarityResults[0].word);
    }
    
    searchSimilarWord("feline");
    ```

47. **Understanding Cosine Similarity:** Cosine similarity measures the cosine of the angle between two vectors. A similarity of 1 means the vectors are identical, while 0 means they are orthogonal (completely different). This measurement is critical for finding semantically similar items.


This section demonstrates how embeddings transform qualitative data into quantitative vectors that you can manipulate mathematically to power search and recommendation features. 
## Conclusion and Next Steps

Vercel’s AI SDK is a powerful, versatile tool that brings a wide array of functionalities into a unified, easy-to-use package. In this comprehensive guide, we have covered:

- **Introduction and Motivation:**  
    Understanding why a unified SDK is necessary for rapid AI development and how it simplifies the integration of multiple LLM providers.
    
- **Installation and Setup:**  
    A step-by-step guide on installing the core package and provider-specific packages, enabling you to start coding immediately.
    
- **Core Components:**  
    Detailed explanations of the Core SDK, AI SDK UI, and AI SDK RSC—each addressing different aspects of AI integration.
    
- **Basic Text Generation and Streaming:**  
    How to generate text responses and stream them in real time, making your applications more interactive.
    
- **Structured Outputs:**  
    Using Zod schemas to define and validate structured outputs from the LLM, ensuring type safety and data consistency.
    
- **Local AI Models:**  
    Steps to integrate and use local AI models through OpenAI-compatible interfaces, giving you flexibility in deployment and testing.
    
- **Tool Calling and AI Agents:**  
    Leveraging the SDK’s capability to let LLMs call external functions, building multi-step reasoning agents that interact with real-world data and services.
    
- **Data Extraction from PDFs:**  
    Techniques for converting unstructured documents into structured, queryable data using LLM-powered extraction.
    
- **Embeddings and Vector Databases:**  
    How to generate and work with embeddings to perform semantic similarity searches, a key feature for search and recommendation systems.
    

By following these detailed steps, you now have the foundational knowledge to build sophisticated AI-powered applications with minimal hassle. Vercel’s AI SDK abstracts away many of the complexities involved in integrating with different language model providers, allowing you to focus on creating innovative solutions.

### What’s Next?

If you’re ready to dive deeper into AI development:

- **Experiment with Different Providers:**  
    Try swapping out OpenAI with Anthropic or even local models to see how easily you can switch between different LLMs.
    
- **Expand Your Tool Set:**  
    Explore additional tool-calling scenarios by integrating with third-party APIs, such as weather services, financial data providers, or even IoT devices.
    
- **Optimize Your Agents:**  
    Use the multi-step reasoning features to build more robust AI agents that handle conversation history, tool feedback loops, and dynamic responses.
    
- **Join the Community:**  
    Visit [AI Hero](https://aihero.com/) for more tutorials, community insights, and advanced tips on AI development. Engaging with a community of developers can provide valuable insights and help you troubleshoot issues as you progress.
    
- **Keep Updated:**  
    Vercel’s AI SDK is under continuous development. Keep an eye on the official documentation and community forums for new features, bug fixes, and best practices.


### Final Thoughts

The journey from building a simple text generator to developing full-fledged AI agents that interact with external tools and process structured data is made remarkably easier with Vercel’s AI SDK. This guide has provided a deep dive into each aspect, from the basics of text generation to advanced topics like tool calling and embedding comparisons. With a solid understanding of these concepts, you are now equipped to innovate and push the boundaries of what’s possible with AI in your own applications.

In the rapidly evolving world of AI, having a flexible and robust toolkit is essential. Vercel’s AI SDK not only speeds up the development process but also empowers you to build intelligent systems that can learn, adapt, and interact with users in meaningful ways.

Thank you for reading this step-by-step guide. By following the detailed instructions provided here, you can unlock the full potential of Vercel’s AI SDK and accelerate your journey into AI-driven application development. **Happy coding!**

