
![alt text](assets/2023_headshot_dunne.jpg "headshot"){: width="200px"}

Hi, I'm Steven Dunne! I am a third year Biophysics PhD student in the  [Rotskoff Group](https://statmech.stanford.edu/). I primarily work on intersections between sampling methods, machine learning, and biophysics.
Previously, I was an undergraduate student at the University of Michigan, where I studied Applied Mathemetics and Biochemistry.
As an undergrad, I worked on a range of research projects, from designing mathematical models for pharmacokinetics in the Rosania Group, to
using molecular dynamics and graph theory to study nanomaterials in the Glotzer group. 




<section id="publications">
  <h2>Publications</h2>
  <ul id="pub-list">Loading…</ul>
</section>

<script>
  fetch('/assets/record.json')
    .then(r => r.json())
    .then(data => {
      const list = document.getElementById('pub-list');
      list.innerHTML = data.map(pub => `
        <li>
          <strong>${pub.title}</strong><br>
          ${pub.authors.map(a => a.name === 'Steven Dunne' ? `<strong>${a.name}</strong>` : a.name).join(', ')} — <em>${pub.venue}</em>, ${pub.year}
          ${pub.citationCount ? ` · Cited by ${pub.citationCount}` : ''}
        </li>
      `).join('');
    });
</script>





<!-- <div style="max-width: 740px; margin: 0 auto;">    
<h2 id="publications">Publications</h2>

<strong>Peer-reviewed</strong><br/>
<ul>
<li class="paper" words="add, your, keywords, here">
    <a href="http://www.mdpi.com/1999-4923/14/1/15">Quantitative Analysis of the Phase 
        Transition Mechanism Underpinning the Systemic Self-Assembly of a Mechanopharmaceutical Device</a> 
    <b>Steven Dunne</b>, Andrew R. Willmer, Rosemary Swanson, Deepak Almeida, Nicole C. Ammerman, Kathleen A. Stringer, Edmund V. Capparelli, Gus R. Rosania
</li>
<li class="paper" words="add, your, keywords, here">
    <a href="http://www.mdpi.com/1999-4923/14/1/17">An Adaptive Biosystems Engineering Approach towards
        Modeling the Soluble-to-Insoluble Phase Transition of Clofazimine</a> 
    Andrew R. Willmer, <b>Steven Dunne</b>, Rosemary Swanson, Deepak Almeida, Nicole C. Ammerman, Kathleen A. Stringer, Edmund V. Capparelli
</li>
<li class="paper" words="add, your, keywords, here">
    <a href="https://www.sciencedirect.com/science/article/pii/S0168365922002929">Molecular design
        of a pathogen activated, self-assembling mechanopharmaceutical device</a>
        Andrew R. Willmer, Jiayi Nie, Mery Vet George De la Rosa, Winnie Wen, <b>Steven Dunne</b>, Gus R. Rosania     
</li>           
</ul> -->

<!-- <strong>Preprints/In Progress</strong><br/>
<ul>
<li class="paper" words="add, your, keywords, here">
    <a href="#"></a>Variational Lifting: Convergent, Exact Sampling with Latent Generative Models
    <b>Steven Dunne</b>, Jérémie Klinger, Alkiviades Boukas, Marylou Gabrié, Grant Rotskoff
    <i>In Preparation</i>
</li>
</ul>
</div> -->
