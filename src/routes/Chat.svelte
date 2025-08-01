<script lang="ts"> 
import {onMount} from "svelte"; 
import {goto} from '@mateothegreat/svelte5-router';
import {authToken} from "../stores/auth"; 
import ChatList from "../components/ChatList.svelte";

function logout() { 
    authToken.remove(); 
    goto ('/login');
}

onMount(() => { 
    if (!$authToken) {
        goto("/login");
    }
});
</script>
<div>
    <div class="container">
        <div class="sidebar">
            <ChatList />
            <button class="logout-button" onclick={logout}>
                ログアウト
            </button>
        </div>
    </div> 
</div>
<style> 
.container {  
    display: flex; 
    height:  100vh;
} 
</style>