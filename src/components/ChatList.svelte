<script lang="ts">
    import { onMount } from "svelte";
    import axios from "axios";
    import { API_HOST } from "../constants";
    import { goto } from '@mateothegreat/svelte5-router';
    import { authToken } from "../stores/auth"; 
    import { get } from "svelte/store";
    
    export let chatId: string | null;   
    
    let chats: { id: string; name: string }[] = [];
    let errorMessage: string | null = null;
    
    async function getData() { 
        try { 
            const token = get(authToken);
            if (!token) {
                errorMessage = "ログインが必要です ";
                return;
            }
            const response = await axios.get(`${API_HOST}/api/v1/chat/`, {
                headers: { Authorization: `Bearer ${token}` }
            });
            chats = response.data.data;
        } catch (error) { 
            errorMessage = "チャットの取得に失敗しました。";
        }
    } 
    
    onMount(async () => {
        await getData(); 
    }); 
    
    function selectChat(chatId: string) { 
        goto(`/${chatId}`);
    }
</script>

{#if errorMessage}
<div class="error">{errorMessage}</div>
{/if} 

<div class="chat-list">
  {#each chats as chat} 
    <button 
      class:selected={chat.id == chatId} 
      on:click={() => selectChat(chat.id)}>
     {chat.name}
    </button>
  {/each}
</div>   

<style>
.selected { 
    background-color: #48c;
    color: white;
}
button { 
    margin-bottom: 10px;
    width: 100%;
}
button:hover { 
    background-color: #48c;
    color: white;
}
.error {
    color: red;
    margin-bottom: 10px;
}
</style>
