<script>
    import { onMount } from "svelte";
  
    let messages = [];
    let newMessage = "";
  
    function sendMessage() {
      if (newMessage.trim() !== "") {
        messages = [...messages, { text: newMessage, sender: "user" }];
        newMessage = "";
        // Simulate a response after a delay
        setTimeout(() => {
          messages = [...messages, { text: "This is a response from ChatGPT.", sender: "bot" }];
        }, 1000);
      }
    }
  
    // Auto-scroll to the bottom when messages update
    let chatContainer;
    onMount(() => {
      const observer = new MutationObserver(() => {
        chatContainer.scrollTop = chatContainer.scrollHeight;
      });
      observer.observe(chatContainer, { childList: true });
    });
  </script>
  
  <style>
    .chat-container {
      display: flex;
      flex-direction: column;
      height: 80vh;
      max-width: 600px;
      margin: auto;
      border: 1px solid #ddd;
      border-radius: 8px;
      overflow: hidden;
    }
    .messages {
      flex: 1;
      padding: 16px;
      overflow-y: auto;
      background: #f9f9f9;
    }
    .message {
      margin: 8px 0;
      padding: 10px;
      border-radius: 8px;
      max-width: 80%;
    }
    .message.user {
      align-self: flex-end;
      background: #007bff;
      color: white;
    }
    .message.bot {
      align-self: flex-start;
      background: #e9ecef;
    }
    .input-container {
      display: flex;
      padding: 16px;
      border-top: 1px solid #ddd;
      background: white;
    }
    .input-container input {
      flex: 1;
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 4px;
      margin-right: 8px;
    }
    .input-container button {
      padding: 10px 16px;
      border: none;
      background: #007bff;
      color: white;
      border-radius: 4px;
      cursor: pointer;
    }
    .input-container button:hover {
      background: #0056b3;
    }
  </style>
  
  <div class="chat-container">
    <div class="messages" bind:this={chatContainer}>
      {#each messages as message}
        <div class="message {message.sender}">
          {message.text}
        </div>
      {/each}
    </div>
    <div class="input-container">
      <input
        type="text"
        bind:value={newMessage}
        on:keydown={(e) => e.key === "Enter" && sendMessage()}
        placeholder="Type your message..."
      />
      <button on:click={sendMessage}>Send</button>
    </div>
  </div>
  