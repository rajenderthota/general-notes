# js function to allow selection 
    javascript:(function() {
    const style = document.createElement('style');
    style.innerHTML = `* { user-select: text !important; -webkit-user-select: text !important; }`;
    document.head.appendChild(style);
    })();
