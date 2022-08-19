# Nomenclator Zoologicus as a Catalogue of Life Data Package (ColDP)

[![DOI](https://zenodo.org/badge/526530074.svg)](https://zenodo.org/badge/latestdoi/526530074)

A mapping between the digitised version of [Nomenclator Zoologicus](http://uio.mbl.edu/NomenclatorZoologicus/) from uBio, and bibliographic databases such as the Biodiversity Heritage Library, CrossRef, and Wikidata.

The original tab-delimited data set was supplied by David Remsen, for background see: Remsen, D. P., Norton, C., & Patterson, D. J. (2006). Taxonomic Informatics Tools for the Electronic Nomenclator Zoologicus. The Biological Bulletin, 210(1), 18–24. https://doi.org/10.2307/4134533

The version is this repository is based on my earlier work [nomenclator-zoologicus](https://github.com/rdmpage/nomenclator-zoologicus), where many bibliographic and other identifiers were added. These identifiers are not included in this version, but will be added back in future.

## Step-by-step guide

The other goal of this work is to learn how to create and publish a Catalogue of Life Data Package (ColDP).

- create a GitHub repository

- create a SQLite database to store and manage the data (you can use whatever data management tool(s) you like. If your database is larger than 100 MB you will have to install [Git Large File Storage (LFS)](https://git-lfs.github.com). Once installed, `git lfs track *.db` will enable GitHub to accept a large SQLite database. Note that Mac users with [Homebrew](https://brew.sh) can simply `brew install git-lfs`.

- create a folder called `code` which is where any scripts to process and/or export the data will be stored.

- export data as CSV or TSV files following the ColDP schema.

- create a metadata.yml file describing the data.

- the release should only include ColDP files so anything else should not be in the release. Add any unwanted files to a file called `.gitattributes`. For example:
```
/code export-ignore
/data export-ignore
*.db export-ignore
*.gitattributes export-ignore
*.gitignore export-ignore
```
- once the first release has been created, we can link to Zenodo and get a DOI, see[Making your project citable](https://coderefinery.github.io/github-without-command-line/doi/). Go to Zenodo and select the GitHub settings. This repository should appear in your list of repositories. Set the switch on the right to “ON”. Create a release for this repository. This should then automatically generate a DOI.

## Database-specific notes

### Exporting data

We export ColDP files using SQL queries.

Export names:
```sql
SELECT 
    id AS ID, 
    genus AS scientificName, 
    author AS authorship, 
    rank,
    genus AS uninomial, 
    code,
    id AS referenceID,
	year AS publishedInYear,
    comments AS remarks
FROM names;
```

Export references:
```sql
SELECT 
	id AS ID,
    publication AS citation
FROM names
WHERE citation <> "";
```

### Related names

The column `related_id` lists the `ID` of a related name. The relationship between `related_id` and `ID` is many to one. For example, the name *Leptusa* (ID 106118) is `related_id` for 44 names, all of which are names originally published as subgenera.  One challenge will be to represent the relationship between `id` and `related_id` using terms recognised by ColDP.



