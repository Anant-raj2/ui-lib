<script>
    import { onMount } from "svelte";

    // Utility functions
    const getDaysInMonth = (year, month) => new Date(year, month + 1, 0).getDate();
    const getFirstDayOfMonth = (year, month) => new Date(year, month, 1).getDay();

    let today = new Date();
    let currentYear = today.getFullYear();
    let currentMonth = today.getMonth();
    let selectedDate = null;

    let daysInMonth = getDaysInMonth(currentYear, currentMonth);
    let firstDayOfMonth = getFirstDayOfMonth(currentYear, currentMonth);

    let highlightedDates = [
        { date: new Date(currentYear, currentMonth, 10), type: "holiday", label: "Holiday" },
        { date: new Date(currentYear, currentMonth, 15), type: "event", label: "Event" },
        { date: new Date(currentYear, currentMonth, 25), type: "birthday", label: "Birthday" },
    ];

    const nextMonth = () => {
        if (currentMonth === 11) {
            currentMonth = 0;
            currentYear += 1;
        } else {
            currentMonth += 1;
        }
        updateCalendar();
    };

    const prevMonth = () => {
        if (currentMonth === 0) {
            currentMonth = 11;
            currentYear -= 1;
        } else {
            currentMonth -= 1;
        }
        updateCalendar();
    };

    const updateCalendar = () => {
        daysInMonth = getDaysInMonth(currentYear, currentMonth);
        firstDayOfMonth = getFirstDayOfMonth(currentYear, currentMonth);
    };

    const selectDate = (day) => {
        selectedDate = new Date(currentYear, currentMonth, day);
    };

    onMount(() => {
        updateCalendar();
    });

    const isHighlighted = (day) => {
        return highlightedDates.find(
            (dateObj) => dateObj.date.getFullYear() === currentYear &&
            dateObj.date.getMonth() === currentMonth &&
            dateObj.date.getDate() === day
        );
    };
</script>

<style>
    .calendar {
        width: 350px;
        margin: 20px auto;
        border: 1px solid #ddd;
        border-radius: 8px;
        box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
        overflow: hidden;
        background-color: #fff;
    }

    .calendar-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        background-color: #007BFF;
        color: white;
        padding: 10px 15px;
        font-size: 18px;
        font-weight: bold;
    }

    .calendar-header button {
        background: none;
        border: none;
        color: white;
        font-size: 18px;
        cursor: pointer;
    }

    .calendar-body {
        padding: 10px;
    }

    .day-names {
        display: grid;
        grid-template-columns: repeat(7, 1fr);
        font-weight: bold;
        text-align: center;
        margin-bottom: 10px;
    }

    .day-names div {
        color: #555;
    }

    .days-grid {
        display: grid;
        grid-template-columns: repeat(7, 1fr);
        text-align: center;
    }

    .day {
        padding: 8px;
        cursor: pointer;
        border-radius: 4px;
        margin: 2px;
        transition: background-color 0.3s ease;
    }

    .day:hover {
        background-color: #f0f0f0;
    }

    .day.selected {
        background-color: #007BFF;
        color: white;
    }

    .day.highlighted {
        background-color: #FFD700;
        color: black;
        position: relative;
    }

    .highlighted .highlight-label {
        position: absolute;
        top: 0;
        right: 0;
        background-color: rgba(0, 0, 0, 0.6);
        color: white;
        font-size: 10px;
        padding: 2px 4px;
        border-radius: 0 4px 0 4px;
    }

    .highlighted.birthday {
        background-color: #FF69B4;
    }

    .highlighted.holiday {
        background-color: #32CD32;
    }

    .highlighted.event {
        background-color: #FFA500;
    }

    .calendar-footer {
        padding: 10px;
        text-align: center;
        font-size: 14px;
        color: #555;
    }
</style>

<div class="calendar">
    <div class="calendar-header">
        <button on:click={prevMonth}>&lt;</button>
        <div>{today.toLocaleString("default", { month: "long" })} {currentYear}</div>
        <button on:click={nextMonth}>&gt;</button>
    </div>
    <div class="calendar-body">
        <div class="day-names">
            <div>Sun</div>
            <div>Mon</div>
            <div>Tue</div>
            <div>Wed</div>
            <div>Thu</div>
            <div>Fri</div>
            <div>Sat</div>
        </div>
        <div class="days-grid">
            {#each Array(firstDayOfMonth).fill(null) as _}
                <div></div>
            {/each}

            {#each Array(daysInMonth).fill(null) as _, dayIndex (dayIndex)}
                {#let day = dayIndex + 1}
                {#let highlight = isHighlighted(day)}
                <div
                    class="day {selectedDate?.getDate() === day && 'selected'} {highlight?.type && `highlighted ${highlight.type}`}"
                    on:click={() => selectDate(day)}
                >
                    {day}
                    {#if highlight}
                        <div class="highlight-label">
                            {highlight.label}
                        </div>
                    {/if}
                </div>
            {/each}
        </div>
    </div>
    <div class="calendar-footer">
        {selectedDate ?
            `Selected Date: ${selectedDate.toDateString()}` :
            "No date selected"}
    </div>
</div>
