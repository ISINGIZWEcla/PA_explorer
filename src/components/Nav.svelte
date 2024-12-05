<script>
    export let activePage = "home"; // This can be passed in to highlight the active link
</script>

<nav>
    <ul>
        <li class="{activePage === 'home' ? 'active' : ''}">
            <a href="/">Home</a>
        </li>
        <li class="{activePage === 'Analytics' ? 'active' : ''}">
            <a href="/Analytics">Analytics</a>
        </li>
        <li class="{activePage === 'About' ? 'active' : ''}">
            <a href="/About">About</a>
        </li>
    </ul>
</nav>

<style>
    nav {
       
        padding: 3rem;
        color: black;
    }
    ul {
        list-style: none;
        margin: 0;
        padding: 0;
        display: flex;
        flex-direction: column;
        gap: 2.5rem;
    }
    li {
        margin: 0;
    }
    a {
        text-decoration: none;
        color: rgb(20, 19, 19);
        font-weight: bolder;
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: larger;
    }
  
    
    .active a {
        font-weight: bold;
        font-size: large;
        color: rgb(90, 90, 86);
    }
</style>
