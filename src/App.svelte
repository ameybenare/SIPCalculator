<script>
	import svelteLogo from "./assets/svelte.svg";
	import Range from "./Range.svelte";
	import { spring, tweened } from "svelte/motion";
	import Pie from "./Pie.svelte";
	

	const store = tweened(0, { duration: 1000 });
	
	$: store.set(percent);
			let investementValue = 1000,
		rateValue = 8,
		timeValue = 8,
		value;
	let amountInvested = investementValue * timeValue * 12;
	let i = rateValue / 12 / 100,
		n = timeValue * 12;
	let x = Math.pow(1 + i, n);
	let finalValue = Math.round(investementValue * ((x - 1) / i) * (1 + i))
	let size = finalValue;
	let totalInvesment = investementValue * timeValue * 12;
	let interest = finalValue - totalInvesment;
	let percent = totalInvesment / finalValue;
	let ratio = (totalInvesment / finalValue) * 100;
	
	let multiplFactor = (
		Math.round((finalValue / totalInvesment) * 100) / 100
	).toFixed(2);

	let theme = "default";

	
	function calculate() {
		(i = rateValue / 12 / 100), (n = timeValue * 12);
		x = Math.pow(1 + i, n);
		finalValue = Math.round(investementValue * ((x - 1) / i) * (1 + i));
		totalInvesment = investementValue * timeValue * 12;
		size = finalValue;

		interest = finalValue - totalInvesment;
		ratio = (totalInvesment / finalValue) * 100;
		//  multiplFactor = Math.trunc(finalValue/totalInvesment);
		multiplFactor = (
			Math.round((finalValue / totalInvesment) * 100) / 100
		).toFixed(2);
		
	}

	let current_task = 0
	let taskPage =true,startTask=true;
	let tasks=[
		{
			description: 'Find the SIP at given time(8 years) and rate(8%), and change only principal amount between 5650 and 5700',
			validate: function(val) {return 761400<val && val<768138;}
		},
		{
			description: 'Find the SIP at given amount and time, change Return rate to 20',
			validate: function(val) {return val==237177;}
		},
		{
			description: 'Get return on Invesement(ROI) greater than 2.(Hint use Rate and Time slider)',
			validate: function(val) {return val>=2;}
		},
		{
			description: 'Get ROI from 1.80 to 1.85 (Hint use Rate and Time slider)',
			validate: function(val) {return val>=1.80 && val<=1.85;}
		}
	]
	let shortTaskDes=[
		{
			description: 'Change only principal amount between 5650 and 5700 and press finish',
			
		},
		{
			description: 'Find the SIP at given amount and time, change interest to 20 ',
		
		},
		{
			description: 'Get return on Invesement(ROI) greater than 2',
		},
		{
			description: 'Get return on Invesement(ROI) in between 1.80 to 1.85',
		}
	]
	let resultPage=false;
	function nextTaskPage(){
		taskPage=true;
		nextTaskButton=false;
		current_task++;
		if(current_task==4){
			resultPage=true;
		}
	}
	$: {
		console.log('multiplFactor:' + multiplFactor	 );
		console.log(timeNeeded);
	}
	let Starttime;
	function nextTask() {
		investementValue = 1000;nextTaskButton=false;
		rateValue = 8;
		timeValue = 8;
		
		(i = rateValue / 12 / 100), (n = timeValue * 12);
		x = Math.pow(1 + i, n);
		finalValue = Math.round(investementValue * ((x - 1) / i) * (1 + i));
		totalInvesment = investementValue * timeValue * 12;
		size = finalValue;

		interest = finalValue - totalInvesment;
		ratio = (totalInvesment / finalValue) * 100;
		//  multiplFactor = Math.trunc(finalValue/totalInvesment);
		multiplFactor = (
			Math.round((finalValue / totalInvesment) * 100) / 100
		).toFixed(2);




		taskPage=false;taskSuccess=false;note = "Press submit button to check answer";
		result="In-complete";
		Starttime=Date.now();
  }
  let timeNeeded;
  let timings=[4];
  let nextTaskButton=false,taskSuccess=false;
  function validate(){
	if(current_task==0 || current_task==1){
		nextTaskButton=tasks[current_task].validate(finalValue);
	}else{
		nextTaskButton=tasks[current_task].validate(multiplFactor);
	}
	
	if(nextTaskButton){
		timeNeeded=Date.now()-Starttime;
		timings.push(timeNeeded);
		taskSuccess=true;
		result="Success";
		note = "";

	}else{
		note = "Press finish button to check answer";
		taskSuccess=false;
		result="In-complete";
	}
  }
  let result="In-complete"
  let note = "Press finish button to check answer";
</script>
<div class="body">
	{#if resultPage}
	<h3>Thank you for your participation</h3>
	
	<div>Please capture the below Result</div>
	<div>Time for task 1 : {timings[1]/1000} seconds</div>
	<div>Time for task 2 : {timings[2]/1000} seconds</div>
	<div>Time for task 3 : {timings[3]/1000} seconds</div>
	<div>Time for task 4 : {timings[4]/1000} seconds</div>
	{:else}
	
	{#if taskPage}
    <div class="heading">SIP Calculator</div>
	<div>You will be asked to perform some tasks. Use the slider input for the same</div>  
	<div>Task: {current_task+1}</div>
	<br>
	<div>{tasks[current_task].description} </div>
	<div>(Press finish button to check answer)</div> 
	
	{:else}
<div class="container">
	<div class="heading">SIP Calculator</div>
	<div><span>
  
		<div class={taskSuccess?'valid':'invalid'}> Task {current_task+1}: {result}</div>
		<div>{shortTaskDes[current_task].description}</div>
		
	  </span></div>
	<div class="innerContainer">
		<div class="pie" style="display: grid;">
			<Pie size={210} percent={ratio} />

			<!--<ul style=" left:100px;list-style-type: none;">
				<li>
					<div class = "circle circle1"></div>
					<output>Interest gained : {interest}</output>
				</li>
				<li>
					<div class = "circle circle2"></div>
					<output>Investmentd amount:  {totalInvesment}</output>
				</li>
			</ul>
		-->
			<div style=" list-style-type: none;position:relative">
				<div>
					<div class="circle circle1" />
					<output style="position: relative;margin-left: -35%;"
						>Returns : € {interest.toLocaleString('en-us')}</output
					>
				</div>
				<div>
					<div class="circle circle2" />
					<output style="position: relative;"
						>Investment amount: € {totalInvesment.toLocaleString('en-us')}</output
					>
				</div>
			</div>
		</div>

		<div class="info">
			<div>
				<output> Monthly Investement</output>
				<input
					type="number"
					min="0"
					max="10000"
					bind:value={investementValue}
					on:change={() => calculate()}
				/>
				<span>
					<Range
						min={0}
						max={10000}
						bind:value={investementValue}
						on:change={() => calculate()}
						on:change={(e) => (value = e.detail.investementValue)}
						id="basic-slider"
					/>
				</span>
			</div>
			<div>
				<output> Return rate PA</output>
				<input
					type="number"
					min="1"
					max="30"
					bind:value={rateValue}
					on:change={() => calculate()}
				/>
				<span>
					<Range
						min={1}
						max={30}
						bind:value={rateValue}
						on:change={() => calculate()}
						on:change={(e1) => (value = e1.detail.rateValue)}
						id="range-slider"
					/>
				</span>
			</div>
			<div>
				<output>Time in years</output>
				<input
					type="number"
					min="1"
					max="20"
					bind:value={timeValue}
					on:change={() => calculate()}
				/>
				<span>
					<Range
						min={1}
						max={20}
						bind:value={timeValue}
						on:change={() => calculate()}
						on:change={(e1) => (value = e1.detail.timeValue)}
						id="time-slider"
					/>
				</span>
			</div>

			<div class="info">
				<span
					><output
						>Final Amount: € <b>{finalValue.toLocaleString('en-us')}</b></output></span><br>
						<span><output>Return on Inves. ROI: <b>{multiplFactor}</b> </output></span>
				
			</div>
			
		</div>
	</div>
	<span>
	<br>
	
		
	<button on:click={validate}>Finish</button></span>


	
</div>

{/if}
{/if}
{#if nextTaskButton  }
<button on:click={nextTaskPage} >Next Task</button>
{/if}
{#if taskPage }
<br>
<button on:click={nextTask} >Start Task</button>
{/if}

<!--<h3>This is for testing</h3>
<div>Your finalValue {finalValue.toLocaleString('en-us')}</div>
<div>Your totalInvesment {totalInvesment.toLocaleString('en-us')}</div>
<div>Your interest {interest.toLocaleString('en-us')} and ration {ratio}</div>
<div>Your multiplFactor {multiplFactor}</div>
<div>Is Slider input efficent for long or short range?</div>
<div>Slider input is more beneficail for accurate or approximate input</div>-->
</div>

<style>
	
	.circle {
		height: 10px;
		width: 10px;
		right: 200px;
		left: -25px;
		top: 20px;
		position: relative;
	}
	.circle1 {
		background-color: cornflowerblue;
	}
	.circle2 {
		background-color: orange;
	}
	.heading {
		font-size: 30px;
		font-style: Robot;
		font-weight: bolder;
		color: #44475b;
		padding: 40px;
		padding-bottom: 60px;
	}
	.pie {
		float: right;
	}
	.info output {
		float: left;
	}
	.info span {
		padding: 20px;
	}
	.info {
		max-width: 70%;
	}
	.info input {
		float: right;
		border-radius: 50px;
		height: 20px;
		font-size: 14px;
		text-align: center;
		width: 80px;
		border-spacing: 50px;
		color: #6185ff;
		border-color: #00d09c;
		opacity: 10;
	}
	.valid{
		color:green;
		font-weight: 600;
	}
	.invalid{
		color:brown;
		font-weight: 600;
	}
	.container {
		width: 900px;
		height: 600px;
		background-color: #ffffff!important;
		box-shadow: 1px 1px 8px 0px grey;
	}
	.innerContainer {
		padding: 10px;
		position: inline;
	}
	button{
		border-color: black;
		
	}
	
</style>
