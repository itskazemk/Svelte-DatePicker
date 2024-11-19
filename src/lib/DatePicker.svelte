<script>
	import DateObject from 'date-object';
	import persian from 'date-object/calendars/es/persian.js';
	import persian_fa from 'date-object/locales/cjs/persian_fa';

	let today = new DateObject({ calendar: persian, locale: persian_fa });

	const firstDayName = new DateObject(today).toFirstOfMonth().weekDay.name;
	const firstDayIndex = new DateObject(today).toFirstOfMonth().weekDay.index;
</script>

<div>
	<input type="text" class="border-2 border-black" />
</div>

<div class="m-2">{today}</div>
<div class="m-2">{firstDayName}</div>
<div class="m-2">{firstDayIndex}</div>

<div class="w-80 !bg-blue-300">
	<div class="flex justify-between">
		before
		<div>{today.year}, {today.month}</div>
		next
	</div>
	<div class="flex justify-between">
		<span>شنبه</span>
		<span>یکشنبه</span>
		<span>دوشنبه</span>
		<span>سه شنبه</span>
		<span>چهارشنبه</span>
		<span>پنجشنبه</span>
		<span>جمعه</span>
	</div>
	<div class="grid grid-cols-7">
		{#each { length: 31 } as _, day}
			<div class="text-center">{day + 1}</div>
		{/each}
	</div>
</div>
