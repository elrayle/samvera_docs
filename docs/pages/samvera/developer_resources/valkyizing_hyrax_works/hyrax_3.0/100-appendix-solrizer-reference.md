---
title: "Appendix: Solrizer Reference"
permalink: valkyrie-work-appendix-solrizer-reference.html
sidebar: home_sidebar
keywords: ["Solrizer"]
last_updated:
version:
  id: hyrax_3.0-stable
  label: 'Hyrax v3.0'
---

### Dynamic Field Names generated using Solrizer

Solrizer, which was used to generate solr field names, is deprecated.  Convert instances of Solrizer.solr_name to hard coded solr field names.  The chart below shows the dynamic field ending to use based on data type (e.g. string, date, etc.) and indexing options (e.g. :facetable, searchable, etc.).

#### Extensions based on data type and indexed as

<table>
  <colgroup>
    <col/>
    <col/>
    <col/>
    <col/>
    <col/>
    <col/>
    <col/>
    <col/>
  </colgroup>
  <tbody>
    <tr>
      <th>data type → <br/>↓indexed as</th>
      <th colspan="1">:date</th>
      <th colspan="1">:time</th>
      <th colspan="1">:text_en</th>
      <th colspan="1">:string</th>
      <th colspan="1">:symbol</th>
      <th colspan="1">:integer</th>
      <th colspan="1">:boolean</th>
    </tr>
    <tr>
      <td colspan="1">none</td>
      <td colspan="1">_dtsim</td>
      <td colspan="1">_dtsim</td>
      <td colspan="1">_tesim</td>
      <td colspan="1">_tesim</td>
      <td colspan="1">_ssim</td>
      <td colspan="1">_isim</td>
      <td colspan="1">_bsi</td>
    </tr>
    <tr>
      <td colspan="1">:stored_searchable</td>
      <td colspan="1">_dtsim</td>
      <td colspan="1">_dtsim</td>
      <td colspan="1">_tesim</td>
      <td colspan="1">_tesim</td>
      <td colspan="1">_ssim</td>
      <td colspan="1">_isim</td>
      <td colspan="1">_bsi</td>
    </tr>
    <tr>
      <td colspan="1">:searchable</td>
      <td colspan="1">_dtim</td>
      <td colspan="1">_dtsim</td>
      <td colspan="1">_teim</td>
      <td colspan="1">_teim</td>
      <td colspan="1">_sim</td>
      <td colspan="1">_iim</td>
      <td colspan="1">_bim</td>
    </tr>
    <tr>
      <td colspan="1">:dateable</td>
      <td colspan="1">_dtsim</td>
      <td colspan="1">_dtsim</td>
      <td colspan="1">_dtsim</td>
      <td colspan="1">_dtsim</td>
      <td colspan="1">_dtsim</td>
      <td colspan="1">_dtsim</td>
      <td colspan="1">_dtsim</td>
    </tr>
    <tr>
      <td colspan="1">:facetable</td>
      <td colspan="1">_sim</td>
      <td colspan="1">_sim</td>
      <td colspan="1">_sim</td>
      <td colspan="1">_sim</td>
      <td colspan="1">_sim</td>
      <td colspan="1">_sim</td>
      <td colspan="1">_sim</td>
    </tr>
    <tr>
      <td colspan="1">:symbol</td>
      <td colspan="1">_ssim</td>
      <td colspan="1">_ssim</td>
      <td colspan="1">_ssim</td>
      <td colspan="1">_ssim</td>
      <td colspan="1">_ssim</td>
      <td colspan="1">_ssim</td>
      <td colspan="1">_ssim</td>
    </tr>
    <tr>
      <td colspan="1">:sortable</td>
      <td colspan="1">_dti</td>
      <td colspan="1">_dti</td>
      <td colspan="1">_tei</td>
      <td colspan="1">_si</td>
      <td colspan="1">_si</td>
      <td colspan="1">_ii</td>
      <td colspan="1">_bi</td>
    </tr>
    <tr>
      <td colspan="1">:stored_sortable</td>
      <td colspan="1">_dtsi</td>
      <td colspan="1">_dtsi</td>
      <td colspan="1">_tesi</td>
      <td colspan="1">_ssi</td>
      <td colspan="1">_ssi</td>
      <td colspan="1">_isi</td>
      <td colspan="1">_bsi</td>
    </tr>
    <tr>
      <td colspan="1">:displayable</td>
      <td colspan="1">_ssm</td>
      <td colspan="1">_ssm</td>
      <td colspan="1">_ssm</td>
      <td colspan="1">_ssm</td>
      <td colspan="1">_ssm</td>
      <td colspan="1">_ssm</td>
      <td colspan="1">_ssm</td>
    </tr>
    <tr>
      <td colspan="1">:unstemmed_searchable</td>
      <td colspan="1">_tim</td>
      <td colspan="1">_tim</td>
      <td colspan="1">_tim</td>
      <td colspan="1">_tim</td>
      <td colspan="1">_tim</td>
      <td colspan="1">_tim</td>
      <td colspan="1">_tim</td>
    </tr>
    <tr>
      <td colspan="1">:simple</td>
      <td colspan="1">_dti</td>
      <td colspan="1">_dti</td>
      <td colspan="1">_tei</td>
      <td colspan="1">_si</td>
      <td colspan="1">_si</td>
      <td colspan="1">_ii</td>
      <td colspan="1">_bi</td>
    </tr>
  </tbody>
</table>

#### Understanding dynamic field extensions

First character or two indicate the type of the field.

<table>
<tr><th width="15%">b</th><td>boolean</td></tr>
<tr><td>dt</td><td>date and/or time</td></tr>
<tr><td>i</td><td>integer</td></tr>
<tr><td>s</td><td>string</td></tr>
<tr><td>te</td><td>English text</td></tr>
</table>

The remaining characters determine how the field is handled in Solr.  The order matters. They appear below in the order they are expected.  All are optional, but at least one should be specified.

<table>
<tr><th width="15%">s</th><td>stored</td><td>indicates that the field value should be part of the retrieved results</td></tr>
<tr><th>i</th><td>indexed</td><td>indicates that the field value can be searched</td></tr>
<tr><th>m</th><td>multiple</td><td>indicates whether or not the field can have multiple values</td></tr>
</table>

Example valid extensions:

<table>
<tr>
  <th width="15%">sim</th>
  <th width="10%">s<br />i<br />m</th>
  <td>- String <br /> 
      - indexed<br /> 
      - multi-valued <br /> 
      NOTE: It will not be part of the returned solr document.</td>
</tr>
<tr>
  <th width="15%">tesim</th>
  <th>te<br/>s<br />i<br />m</th>
  <td>- English text <br /> 
      - stored<br /> 
      - indexed<br /> 
      - multi-valued</td>
</tr>
<tr>
  <th width="15%">dtsi</th>
  <th>dt<br />s<br />i</th>
  <td>- Date <br /> 
      - stored<br /> 
      - indexed<br /> 
      NOTE: It cannot have multiple values.</td>
</tr>
</table>

<br>
<hr>
