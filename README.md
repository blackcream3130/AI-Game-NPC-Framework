# AI-Game-NPC-Framework
A system that utilizes Docker and PostgreSQL to allow NPCs to store and retrieve past conversations (memories) with the user


# Overview
Many indie developers struggle to implement AI-driven NPCs due to complex backend setups and memory management. This project bridges the gap by providing a ready-to-use template for **Unity (C#)** and **Roblox**, backed by a scalable **Dockerized PostgreSQL** database. 

Our goal is to make immersive AI interactions accessible to all game developers.

# Key Features
*   **Cross-Engine Support:** Ready-to-use client scripts for Unity (C#) and Roblox.
*   **Persistent NPC Memory:** Utilizes PostgreSQL and Docker to store and retrieve player-NPC chat histories, allowing NPCs to "remember" past interactions.
*   **Persona Injection System:** Easily configure unique personalities. (e.g., You can easily set up a prompt to make an NPC act like a specific anime character, using distinct speech patterns).
*   **Lightweight Gateway:** Built on customized backend frameworks (inspired by OpenClaw) for low-latency API routing.

# Architecture
1.  **Client (Game Engine):** Captures player input and handles in-game events.
2.  **API Gateway:** Receives requests, manages environment variables (`DATABASE_URL`, `OPENAI_API_KEY`), and constructs prompts.
3.  **Database (Memory):** PostgreSQL stores conversational context. 
4.  **LLM (OpenAI):** Generates contextual, persona-driven dialogue.

# Quick Start (Backend Setup)
```bash
# 1. Clone the repository
git clone [https://github.com/blackcream/AI-NPC-Framework.git](https://github.com/blackcraem/AI-NPC-Framework.git)

# 2. Configure Environment Variables
# Create a .env file and add your OpenAI API key and Database URL
OPENAI_API_KEY="your_api_key_here"
DATABASE_URL="postgresql://user:password@localhost:5432/npc_memory"

# 3. Run the Docker container for the database
docker-compose up -d
```

# How to Configure NPC Personas (Example)

You can easily inject unique personalities, lore, and speech patterns into your NPCs by simply passing a system prompt. No complex coding is required.

### 1. Define the Persona Prompt
Here is an example of giving an NPC a highly specific personality with a unique speech quirk:

> "Act as a highly intelligent but curious young clone girl. Your defining trait is always appending a descriptive tag about your emotions or actions at the end of your sentences. You must maintain this unique third-person speech pattern, such as: '...says Misaka as Misaka proudly puffs out her chest!' Greet the player with an inquisitive attitude."

### 2. Apply via Code (Unity C#)
Pass this prompt to the NPC Manager. The backend will automatically apply this persona to the conversation context.

```csharp
using UnityEngine;
using OpenAIGameFramework;

public class NPCController : MonoBehaviour
{
    private string npcId = "Clone_NPC_001";
    private string personaPrompt = "Act as a highly intelligent but curious young clone girl... (omitted) ...such as: '...says Misaka as Misaka...'";

    void Start()
    {
        // 1. Initialize the NPC with its unique ID and persona prompt
        NPCManager.Instance.InitializeNPC(npcId, personaPrompt);
    }

    // Called when the player interacts with the NPC
    public async void OnPlayerSpeak(string playerMessage)
    {
        // 2. Fetch the asynchronous response reflecting the persona and past memories
        string npcResponse = await NPCManager.Instance.SendMessageToNPC(npcId, playerMessage);
        
        Debug.Log($"NPC: {npcResponse}");
        // Example Output: "You must be a new adventurer! Nice to meet you, says Misaka as Misaka smiles brightly!"
    }
}
```

