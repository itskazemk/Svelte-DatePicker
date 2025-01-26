<script lang="ts">
	import {
		CalendarDate,
		startOfWeek,
		toCalendar,
		fromDate,
		isSameMonth,
		createCalendar,
		getLocalTimeZone,
		PersianCalendar,
		ZonedDateTime,
		toCalendarDate
	} from '@internationalized/date';
	import { ChevronRight, ChevronLeft } from 'lucide-svelte';

	// let { value = $bindable() } = $props();
	let { value = $bindable() } = $props();

	// تاریخ امروز رو میسازیم (به میلادی)
	// create new ZonedDateTime (like new Date())
	let date = fromDate(new Date(), 'Asia/Tehran');

	// زمان رو از تاریخ امروز حذف میکنیم و سپس اون رو به شمسی تبدیل میکنیم
	// remove time from date and then change the calendar from grogrian to persian
	let today = $state(toCalendar(toCalendarDate(date), new PersianCalendar()));

	// در صورتی که استیتی به ما پاس داده نشده باشد از تاریخ امروز برای نمایش ماه تقویم استفاده میکنیم
	let intialMonth = value ? value : today;

	let selectedDate = $state<CalendarDate | null>(value);
	let currentMonth = $state<CalendarDate>(intialMonth.set({ day: 1 }));

	$effect(() => (value = selectedDate));

	const weekDays = ['ش', 'ی', 'د', 'س', 'چ', 'پ', 'ج'];

	// Date formatter for Persian locale
	const formatter = new Intl.DateTimeFormat('fa-IR', {
		calendar: 'persian',
		year: 'numeric',
		month: 'long',
		day: 'numeric'
	});

	// Generate 6-week calendar grid
	let calendarDays = $derived(generateCalendar(currentMonth));

	function generateCalendar(monthStart) {
		const firstDayOfMonth = monthStart.set({ day: 1 });
		const gridStart = startOfWeek(firstDayOfMonth, 'fa-IR');
		return Array.from({ length: 42 }, (_, i) => gridStart.add({ days: i }));
	}

	function selectDate(date) {
		if (!isSameMonth(date, currentMonth)) return;
		selectedDate = date;
	}

	function changeMonth(offset) {
		currentMonth = currentMonth.add({ months: offset });
	}

	let formattedDate = $derived(
		selectedDate ? formatter.format(selectedDate.toDate()) : 'تاریخ انتخاب نشده'
	);
</script>

<div class="jalali-calendar" dir="rtl">
	<!-- Header -->
	<div class="header">
		<button onclick={() => changeMonth(-1)}><ChevronRight /></button>
		<h3>{formatter.format(currentMonth.toDate())}</h3>
		<button onclick={() => changeMonth(1)}><ChevronLeft /></button>
	</div>

	<!-- Weekday headers -->
	<div class="weekdays">
		{#each weekDays as day}
			<div>{day}</div>
		{/each}
	</div>

	<!-- Calendar grid -->
	<div class="grid">
		{#each calendarDays as date}
			<button
				class:other-month={!isSameMonth(date, currentMonth)}
				class:current-month={isSameMonth(date, currentMonth)}
				class="hover:!bg-red-500"
				onclick={() => selectDate(date)}
			>
				{date.day}
			</button>
		{/each}
	</div>

	<!-- Selected date display -->
	<div class="selected-date">{formattedDate}</div>
</div>

<style>
	.jalali-calendar {
		direction: rtl;
		font-family: Tahoma, sans-serif;
		width: 300px;
		border: 1px solid #ddd;
		border-radius: 4px;
		padding: 1rem;
	}

	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 1rem;
	}

	.weekdays {
		display: grid;
		grid-template-columns: repeat(7, 1fr);
		gap: 0.25rem;
		margin-bottom: 0.5rem;
		font-weight: bold;
	}

	.grid {
		display: grid;
		grid-template-columns: repeat(7, 1fr);
		gap: 0.25rem;
	}

	button {
		padding: 0.5rem;
		border: none;
		background: #f8f8f8;
		border-radius: 4px;
		cursor: pointer;
	}

	.current-month {
		background: white;
	}

	.other-month {
		background: gray;
	}

	.selected {
		background: #2196f3;
		color: white;
	}

	.selected-date {
		margin-top: 1rem;
		text-align: center;
		font-weight: bold;
	}
</style>
