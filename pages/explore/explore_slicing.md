  <h1 id="general-guidance">General Guidance</h1>

<p>Slicing is a method of constraining profiles. Slicing is a way of splitting lists into sublists that are distinguished from each other by defining a discriminator. For more information about slicing see the <a href="https://www.hl7.org/fhir/profiling.html#slicing">HL7 Profiling page</a>.</p>

<p>A discriminator has 3 parts</p>
<ul>
  <li>the discriminating value (CareConnect &amp; NHS Digital profiles don’t currently support exist, pattern, type &amp; profile discriminators). This is used to ensure that each slice are distinguishable from each other</li>
  <li>an Ordering attribute that is used to order the slices in a FHIR instance. In CareConnect and NHS Digital profiles this is almost always set to “Ordering: false”</li>
  <li>a Rules attribute to state whether the slicing is Open (further information may be added outside the discriminator definition) or Closed (only data that conforms to the discriminator may be present).</li>
</ul>

<p>In CareConnect and NHS Digital profiles, slicing is used in various way, including</p>
<ul>
  <li>identifying components within a blood pressure Observation to fix codes for systolic and diastolic pressure values. See <a href="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-BloodPressure-Observation-1">CareConnect-BloodPressure-1</a>.</li>
  <li>exemplifying commonly used identification schemes used in the NHS. See the identifier slice in <a href="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1">CareConnect-Organization-1</a> showing how to use ODS codes.</li>
  <li>exemplifying commonly used terminology, such as SNOMED CT. See the snomedCT slicing in <a href="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Procedure-1">CareConnect-Procedure-1</a>. Note that the snomedCT slice also includes a snomedCTDescriptionID extension that is only relevant to SNOMED.</li>
  <li>identifying sections in documents to allow section.code values to be fixed. See <a href="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Composition-1">CareConnect-Composition-1</a>
</li>
</ul>

<h1 id="examples">Examples</h1>

<h2 id="1-slicing-on-an-codeableconcept-01">1. Slicing on an CodeableConcept [0..1]</h2>
<p><img src="./images/Slicing-Example1.png"><br>
In this example, the method element has been sliced to include a SNOMED CT slice. The slice allows the Extension-coding-sctdescid to be used within the SNOMED slice. The snomedCT slice is optional, so any coding system would be legal. As method has cardinality [0..1], only one CodeableConcept is allowed. However, multiple codings are allowed within the CodeableConcept to allow for translations between codeSystem.</p>

<p>A valid snippet example using the snomed CT slice</p>
<div class="highlighter-rouge"><div class="highlight"><pre class="highlight"><code>&lt;method&gt;
	&lt;coding&gt;
		&lt;system value="http://snomed.info/sct"/&gt;
		&lt;code value="709497004"/&gt;
		&lt;display value="Assessment of peristomal skin"/&gt;
	&lt;/coding&gt;
&lt;/method&gt;
</code></pre></div></div>
<p>A valid snippet example using the SNOMED CT slice and description extension</p>
<div class="highlighter-rouge"><div class="highlight"><pre class="highlight"><code>&lt;method&gt;
	&lt;coding&gt;
		&lt;extension url="https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-coding-sctdescid"&gt;
			&lt;extension url="descriptionId"&gt;
				&lt;valueId value="338701010"/&gt;
			&lt;/extension&gt;
			&lt;extension url="descriptionDisplay"&gt;
				&lt;valueString value="Assessing behaviour"/&gt;
			&lt;/extension&gt;
		&lt;/extension&gt;	
		&lt;system value="http://snomed.info/sct"/&gt;
		&lt;code value="225385005"/&gt;
		&lt;display value="Behavioral assessment"/&gt;
	&lt;/coding&gt;
&lt;/method&gt;
</code></pre></div></div>
<p>A valid snippet example using the SNOMED CT slice with a Read translation (that was the code that was originally selected)</p>
<div class="highlighter-rouge"><div class="highlight"><pre class="highlight"><code>&lt;method&gt;
	&lt;coding&gt;
		&lt;system value="http://snomed.info/sct"/&gt;
		&lt;code value="703749006"/&gt;
		&lt;display value="Infant Behavioural Assessment and Intervention Programme"/&gt;
	&lt;/coding&gt;
	&lt;coding&gt;
		&lt;system value="2.16.840.1.113883.2.1.6.2"/&gt;
		&lt;code value="8GF5"/&gt;
		&lt;display value="IBAIP"/&gt;
		&lt;userSelected="true"/&gt;
	&lt;/coding&gt;
&lt;/method&gt;
</code></pre></div></div>

<h2 id="2-slicing-on-an-codeableconcept-0-or-1">2. Slicing on an CodeableConcept [0..*] or [1..*]</h2>
<p><img src="./images/Slicing-Example2.png"><br>
Manifestation allows for multiple manifestations to be recorded. Further, manifestation includes a snomedCT slice. In this scenario, each manifestation is recorded within their own <manifestation> tags.</manifestation></p>

<p>A valid snippet showing manifestation findings of swelling and a blanching rash</p>
<div class="highlighter-rouge"><div class="highlight"><pre class="highlight"><code>&lt;manifestation&gt;
	&lt;coding&gt;
		&lt;system value="http://snomed.info/sct"/&gt;
		&lt;code value="299037003"/&gt;
		&lt;display value="Swelling of hand"/&gt;
	&lt;/coding&gt;
&lt;/manifestation&gt;
&lt;manifestation&gt;
	&lt;coding&gt;
		&lt;system value="http://snomed.info/sct"/&gt;
		&lt;code value="400990009"/&gt;
		&lt;display value="Blanching rash"/&gt;
	&lt;/coding&gt;
&lt;/manifestation&gt;
</code></pre></div></div>
<p>Note that the SNOMED description extension and translations may be used in the recorded manifestations as above.</p>
