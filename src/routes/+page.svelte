<svelte:head>
	<title>Unscroll - Replace scrolling with better habits</title>
</svelte:head>

<script lang="ts">
    import TypedJs from "$lib/TypedJS.svelte";
    import habits from "$lib/habits.json"

    let wastedminutes:number;
    let selectedHabit:any;
    let timeperiod:number;
    let tasksCompleted:number;
    let wastedminutesDiv:HTMLDivElement;
    let timeperiodDiv:HTMLDivElement;
    let activityDiv:HTMLDivElement;
    let resultDiv:HTMLDivElement;

    $: if (wastedminutes && selectedHabit && timeperiod) {
        tasksCompleted = Math.round((wastedminutes * selectedHabit.speed * timeperiod) / selectedHabit.accomplishment_length);
    }
</script>


<main class="bg-zinc-100 text-zinc-800 text-lg lg:text-2xl font-bold leading-10">

    <!-- BRAND -->
    <a href="/" class="absolute top-10 left-10 z-50 font-bold text-xl tracking-wide hover:scale-110 hover:-rotate-2 transition-all border px-5 py-2 bg-zinc-100 rounded">📱 Unscroll</a>

    <!-- HERO -->
    <div class="relative w-full h-[80vh] bg-[url('1.jpg')] bg-cover bg-center">        
        <div class="hero text-zinc-100 text-2xl lg:text-4xl font-bold absolute top-1/2 mx-10">
            <div>Replace mindless scrolling with</div>
            <div class="text-4xl lg:text-6xl mt-5"><TypedJs initialString="better habbits." strings={[...habits.map(habit => habit.activity + '.'), 'better habbits.']} /></div>
        </div>
    </div>
    
    <!-- SELECT WASTED MINUTES -->
    <div bind:this={wastedminutesDiv} class="relative bg-[#8DCCFF] py-20 lg:py-40 px-10 text-center lg:text-left my-20 mx-10 md:mx-40 lg:mx-60 rounded-md group scroll-m-40">
        <span>I scroll <span class="underline">reels/tik-tok/shorts</span> for</span>
        <select bind:value={wastedminutes} on:change={() => timeperiodDiv.scrollIntoView({ behavior: "smooth" })} class="bg-transparent/10 p-2 rounded outline-none hover:bg-transparent/20 transition-all">
            <option disabled selected>choose</option>
            <option value="10">10 minutes</option>
            <option value="30">30 minutes</option>
            <option value="60">1 hour</option>
            <option value="300">5 hours</option>
            <option value="600">10 hours</option>
        </select>
        <span>a day.</span>

        <div class="absolute top-0 left-1/2 -translate-x-1/2 -translate-y-1/2 lg:top-1/2 lg:left-auto lg:right-0 lg:translate-x-1/2 text-7xl lg:text-9xl drop-shadow-xl group-hover:scale-110 group-hover:-rotate-3 transition-all">😟</div>
    </div>
    
    <!-- SELECT TIMEPERIOD -->
    {#if wastedminutes > 0}
        <div bind:this={timeperiodDiv} class="relative bg-[#B31B81] py-20 lg:py-40 px-10 text-center lg:text-left my-20 mx-10 md:mx-40 lg:mx-60 rounded-md group scroll-m-40">
            <span>Instead, in</span> 
            <select bind:value={timeperiod} on:change={() => activityDiv.scrollIntoView({ behavior: "smooth" })} class="bg-transparent/10 p-2 rounded outline-none hover:bg-transparent/20 transition-all">
                <option disabled selected>choose</option>
                <option value="30">1 month</option>
                <option value="90">3 months</option>
                <option value="365">1 year</option>
                <option value="3650">10 years</option>
                <option value="36500">100 years</option>
            </select>
            
            <div class="absolute top-0 left-1/2 -translate-x-1/2 -translate-y-1/2 lg:top-1/2 lg:left-auto lg:right-0 lg:translate-x-1/2 text-7xl lg:text-9xl drop-shadow-xl group-hover:scale-110 group-hover:-rotate-3 transition-all">⏳</div>
        </div>
    {/if}
    
    <!-- SELECT ACTIVITY -->
    {#if timeperiod > 0}
        <div bind:this={activityDiv} class="relative bg-[#F8AF12] py-20 lg:py-40 px-10 text-center lg:text-left my-20 mx-10 md:mx-40 lg:mx-60 rounded-md group scroll-m-40">
            <span>I can</span> 
            <select bind:value={selectedHabit} on:change={() => resultDiv.scrollIntoView({ behavior: "smooth" })} class="bg-transparent/10 p-2 rounded outline-none hover:bg-transparent/20 transition-all">
                <option disabled selected>choose</option>
                {#each habits as habit}
                    <option value={habit}>{habit.name} {habit.emoji}</option>
                {/each}
            </select>

            <div class="absolute top-0 left-1/2 -translate-x-1/2 -translate-y-1/2 lg:top-1/2 lg:left-auto lg:right-0 lg:translate-x-1/2 text-7xl lg:text-9xl drop-shadow-xl group-hover:scale-110 group-hover:-rotate-3 transition-all">🤩</div>
        </div>
    {/if}
    
    <!-- ACCOMPLISHMENT RESULT -->
    {#if tasksCompleted}
        <div bind:this={resultDiv} class="relative bg-[#a6a8b7] py-20 lg:py-40 px-10 text-center lg:text-left my-20 mx-10 md:mx-40 lg:mx-60 rounded-md group scroll-m-40">
            <div class="text-sm md:text-base lg:text-xl font-normal italic mb-5 ">Scrolling <span class="font-bold">{wastedminutes} mins</span> a day for <span class="font-bold">{timeperiod} days</span> would amount to same time as to {selectedHabit.name}</div>
            <div class="text-7xl lg:text-9xl">{tasksCompleted}</div>
            <div class="text-2xl">{selectedHabit.accomplishment}</div>
            <div class="absolute top-0 left-1/2 -translate-x-1/2 -translate-y-1/2 lg:top-1/2 lg:left-auto lg:right-0 lg:translate-x-1/2 text-7xl lg:text-9xl drop-shadow-xl group-hover:scale-110 group-hover:-rotate-3 transition-all">{selectedHabit.emoji}</div>
            <!-- <div>❕Calculated at average {selectedHabit.activity} speed of {selectedHabit.speed} {selectedHabit.accomplishment_unit} per minute</div> -->
        </div>
    {/if}

    <footer class="mt-20 bg-zinc-300 text-center pt-3 pb-2 text-sm font-normal">Made by <a href="https://x.com/friesnwaffle" target="_blank" class="underline">Prince</a> 😎</footer>
</main>


<style>
    * {
        font-family: "Josefin Sans", sans-serif;
    }
</style>