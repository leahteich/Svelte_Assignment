<script>
    var today = new Date();
    var dd = String(today.getDate()).padStart(2, '0');
    var mm = String(today.getMonth() + 1).padStart(2, '0'); //January is 0!
    var yyyy = today.getFullYear();

    today = mm + '/' + dd + '/' + yyyy;

    // #Properties: Used export keyword to declare properties of the toolbar. 
    export let myDate = today

</script>


<p>This is a toolbar! Feel free to reuse me on other parts of this site. Today's date is {myDate}. </p>
