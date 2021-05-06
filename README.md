# notesApp
App using CSS, HTML, and JS for note taking.

This project creates a stand alone note taking app which can perform the CRUD operations and save/pull data from local storage.

This can be utilized in a multitude of situations and edited to fit the desired scheme.

Building this app helped me to practice and learn better the following:
1) Using libraries (marked - https://github.com/markedjs/marked?utm_source=cdnjs&utm_medium=cdnjs_link&utm_campaign=cdnjs_library)
2) DOM manipulation
3) Creating & implementing logic
4) Working with local storage

A general walkthrough of the JavaScript code is below.

Add initial selectors.
```JavaScript
const notesEl = document.querySelector('.notes');
const editBtn = document.querySelector('.edit');
const deleteBtn = document.querySelector('.delete');

const main = notesEl.querySelector('.main');
const textArea = notesEl.querySelector('textarea');
```

Add on event listeners to the text area and edit buttons.
```JavaScript
editBtn.addEventListener('click', () => {
    main.classList.toggle('hidden');
    textArea.classList.toggle('hidden');
});

textArea.addEventListener('input', (e) => {
    const { value } = e.target;

    main.innerHTML = marked(value);
})
```

Add a selector and event listener to the add note button.
```JavaScript
const addBtn = document.getElementById('add');

addBtn.addEventListener('click', () => {
    addNewNote();
});
```

Create the addNewNote() function and add an event listener to the delete button. Place everything not related to the add note button inside of the new addNewNote() function.
```JavaScript

function addNewNote(text = ''){
    const note = document.createElement('div');
    note.classList.add('note');

    note.innerHTML = `
    <div class="notes">
    <div class="tools">
        <button class="edit"><i class="fas fa-edit"></i></button>
        <button class="delete"><i class="fas fa-trash"></i></button>
    </div>
        <div class="main hidden">
        
    </div>
        <textarea></textarea>
    </div> `;

    const editBtn = note.querySelector('.edit');
    const deleteBtn = note.querySelector('.delete');

    const main = note.querySelector('.main');
    const textArea = note.querySelector('textarea');

    editBtn.addEventListener('click', () => {
        main.classList.toggle('hidden');
        textArea.classList.toggle('hidden');
    });

    deleteBtn.addEventListener('click', () => {
        note.remove();
        updateLS();
    })

    textArea.addEventListener('input', (e) => {
        const { value } = e.target;

        main.innerHTML = marked(value);
    });

    document.body.appendChild(note);
};
```

Create the updateLS() function to save new and edited data into local storage.
```JavaScript
function updateLS() {
    const notesText = document.querySelectorAll('textarea');

    const notes = [];

    notesText.forEach(note => {
        notes.push(note.value);
    });
    localStorage.setItem('notes', JSON.stringify(notes));
}
```

Add a selector to JSON data inside of local storage, convert JSON back to JavaScript using .parse(). Set the value of textArea equal to text.
```JavaScript
const notes = JSON.parse(localStorage.getItem('notes'));
if(notes){
    notes.forEach(note => {
        addNewNote(note);
    })
}

   textArea.value = text;
   main.innerHTML = marked(text);
```

***End walkthrough
