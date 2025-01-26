<script>
	import Calendar from '$lib/Calendar.svelte';
	import {
		CalendarDate,
		fromDate,
		getLocalTimeZone,
		PersianCalendar,
		toCalendar,
		toCalendarDate
	} from '@internationalized/date';

	let date = $state();

	$inspect(date);
</script>

<!-- <Calendar bind:value={date} /> -->
<Calendar bind:value={date} />

<div>{date && date.toString()}22</div>
