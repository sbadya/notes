<template>
	<div class="wrapper">
		<div class="wrapper-content">
			<section>
				<div class="container">
					<message v-if="message" :message="message" />
					<new-note @addNote="addNote" />
					<div class="notes-header">
						<h1 class="notes-header__title">{{ title }}</h1>
						<search
							:value="search"
							placeholder="Find this note"
							@search="search = $event"
						/>
						<div class="notes-header__icons">
							<svg
								:class="{
									'notes-header__icon': true,
									'notes-header__icon_active': grid
								}"
								@click="toggleView(true)"
								xmlns="http://www.w3.org/2000/svg"
								width="24"
								height="24"
								viewBox="0 0 24 24"
								fill="none"
								stroke="currentColor"
								stroke-width="2"
								stroke-linecap="round"
								stroke-linejoin="round"
							>
								<rect x="3" y="3" width="7" height="7"></rect>
								<rect x="14" y="3" width="7" height="7"></rect>
								<rect x="14" y="14" width="7" height="7"></rect>
								<rect x="3" y="14" width="7" height="7"></rect>
							</svg>
							<svg
								:class="{
									'notes-header__icon': true,
									'notes-header__icon_active': !grid
								}"
								@click="toggleView()"
								xmlns="http://www.w3.org/2000/svg"
								width="24"
								height="24"
								viewBox="0 0 24 24"
								fill="none"
								stroke="currentColor"
								stroke-width="2"
								stroke-linecap="round"
								stroke-linejoin="round"
							>
								<line x1="8" y1="6" x2="21" y2="6"></line>
								<line x1="8" y1="12" x2="21" y2="12"></line>
								<line x1="8" y1="18" x2="21" y2="18"></line>
								<line x1="3" y1="6" x2="3" y2="6"></line>
								<line x1="3" y1="12" x2="3" y2="12"></line>
								<line x1="3" y1="18" x2="3" y2="18"></line>
							</svg>
						</div>
					</div>
					<notes
						:notes="visibleNotes"
						:grid="grid"
						@removeNote="(note) => removeNote(note)"
					/>
				</div>
			</section>
		</div>
	</div>
</template>

<script>
	import message from '@/components/Message.vue';
	import newNote from '@/components/NewNote.vue';
	import notes from '@/components/Notes.vue';
	import search from '@/components/Search.vue';

	const serverData = [
		{
			title: 'First Note',
			descr: 'Description for first note',
			date: new Date(Date.now()).toLocaleString(),
			priority: 'high'
		},
		{
			title: 'Second Note',
			descr: 'Description for second note',
			date: new Date(Date.now()).toLocaleString(),
			priority: 'very-high'
		},
		{
			title: 'Third Note',
			descr: 'Description for third note',
			date: new Date(Date.now()).toLocaleString(),
			priority: 'normal'
		}
	];

	export default {
		components: {
			message,
			newNote,
			notes,
			search
		},
		computed: {
			visibleNotes() {
				const search = this.search.toLowerCase();
				const notes = this.notesList.filter((note) => {
					const title = note.title.toLowerCase();
					const show = title.startsWith(search);

					return show;
				});

				return notes;
			}
		},
		data() {
			return {
				message: '',
				title: 'Notes App',
				notesList: [...serverData],
				grid: true,
				search: ''
			};
		},
		methods: {
			addNote(note) {
				const { title, descr, priority } = note;
				if (!title) {
					this.message = 'Title can not be empty!';
					return;
				}

				const date = new Date(Date.now()).toLocaleString();
				this.notesList = [...this.notesList, { title, descr, date, priority }];
				this.message = '';
			},
			removeNote(removedNote) {
				const restNotes = this.notesList.filter((note) => note !== removedNote);
				this.notesList = restNotes;
			},
			toggleView(showGrid = false) {
				this.grid = showGrid;
			}
		}
	};
</script>
