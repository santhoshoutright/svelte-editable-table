<script>
  import Table from './Table.svelte';
  
  const headings = [
    {title: 'Scout', property: 'scoutName',  width: 75},
    {title: 'Grower', property: 'growerName',  width: 75},
    {title: 'Field', property: 'fieldName', width: 200},
    {title: 'Acres', property: 'acres', type: 'number', width: 100}
  ];

  const data = [
    {scoutName: 'S1', growerName: 'G1', fieldName: 'F1', acres: 100},
    {scoutName: 'S1', growerName: 'G2', fieldName: 'F2', acres: 200},
    {scoutName: 'S2', growerName: 'G3', fieldName: 'F3', acres: 300},
    {scoutName: 'S2', growerName: 'G4', fieldName: 'F4', acres: 400}
  ];

  // console.log(" headings !!");
  // console.log(headings);
</script>

<main>
  <h1>Next 7 Days</h1>
  <Table {headings} {data} />
</main>

<style>
</style>
