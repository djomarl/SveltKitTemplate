<script lang="ts">
    import { page } from '$app/stores';
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';

    let countdown = 30;
    let intervalId: number;

    function goBack() {
        if (typeof window !== 'undefined' && window.history.length > 1) {
            window.history.back();
        } else {
            goto('/');
        }
    }

    onMount(() => {
        intervalId = setInterval(() => {
            countdown--;
            if (countdown <= 0) {
                clearInterval(intervalId);
                goBack();
            }
        }, 1000);

        return () => {
            if (intervalId) {
                clearInterval(intervalId);
            }
        };
    });
</script>

<div class="min-h-screen flex items-center justify-center bg-gradient-to-br from-purple-500 to-indigo-600 p-8">
    <div class="bg-white rounded-xl shadow-2xl p-8 text-center max-w-md w-full">

        <img src="https://i.imgur.com/5hO6yxS.gif" alt="Loading..." class="mx-auto mb-4 w-100 h-100" />
        
        <h1 class="text-3xl font-bold text-gray-800 mb-4">
            {$page.status}
        </h1>
        
        <p class="text-lg text-red-500 font-semibold mb-4">
            {$page.error?.message || 'Er is een onbekende fout opgetreden'}
        </p>
        
        <p class="text-gray-600 mb-6">
            Je wordt automatisch teruggeleid over <span class="font-bold text-purple-600">{countdown}</span> seconden
        </p>
        
        <div class="flex flex-col sm:flex-row gap-3 justify-center">
            <button 
                on:click={goBack}
                class="bg-purple-500 hover:bg-purple-600 text-white font-semibold py-3 px-6 rounded-lg transition-all duration-200 hover:-translate-y-1 hover:shadow-lg flex items-center justify-center gap-2"
            >
                ← Nu terug
            </button>
        </div>
    </div>
</div>