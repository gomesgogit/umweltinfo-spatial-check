\# Where is the data, and can you trust its location?



A spatial metadata check for search results from \[umwelt.info](https://umwelt.info), the German environmental data portal.



When you search umwelt.info for "Wald AND Brandenburg", do the results really lie in Brandenburg? This notebook downloads the search results from the umwelt.info API, draws the bounding box of each dataset on a map, and checks whether the box lies in the region  asks for.



\## What it does



1\. downloads the first 200 results for a query and saves them as a snapshot (the live API changes daily so the result could be different daily without the snapshot of the day of search),

2\. turns each bounding box into a shape,

3\. compares each box with the Brandenburg outline from VG250 (BKG),

4\. puts each box into one of three categories: in Brandenburg, near Brandenburg (incl. Berlin), elsewhere,

5\. saves the results (GeoPackage, CSV) and creates an interactive map.



\## Results (snapshot of 7 Oct 2026, 200 results)



| Category | Records | Share |

|---|---|---|

| In Brandenburg | 135 | 67.5 % |

| Near Brandenburg (incl. Berlin) | 49 | 24.5 % |

| Elsewhere | 13 | 6.5 % |

| No bounding box | 3 | 1.5 % |



About two thirds of the results really lie in Brandenburg. About one in four lies near it, mostly Berlin and map sheets on the state border. 13 results appear in a Brandenburg search although their box lies elsewhere (zoom out and see the purple boxes).



\## Data



| Dataset | Source | License |

|---|---|---|

| Search results (metadata) | umwelt.info API, via \[umwelt-apy](https://pypi.org/project/umwelt-apy/) | see each record |

| VG250 administrative boundaries | \[BKG](https://gdz.bkg.bund.de) | dl-de/by-2-0 |

| TopPlusOpen basemap | BKG | dl-de/by-2-0 |



The snapshot is included in `data/`. VG250 is not included because of its size.



\## How to run



1\. Create the environment:



2\. Download VG250 (GeoPackage) from the BKG and unzip it into `data/`. The notebook finds `DE\_VG250.gpkg` anywhere inside that folder.



3\. Open `notebook/spatial\_metadata\_check.ipynb` in JupyterLab and run all cells.



The notebook is set up for Brandenburg. For another state, change `QUERY` and `STATE\_KEY` in the setup cell, delete or rename the snapshot file so a new one is downloaded, and adjust the category names and the map center, which still say Brandenburg.



\## Project structure

├── data/results\_2026-10-07.json snapshot of the search results

├── notebook/spatial\_metadata\_check.ipynb

├── outputs/ results (GeoPackage, CSV, HTML map)

├── environment.yml

└── README.md





\## Limitations



\- The result is a snapshot: one query, the first 200 results, one day.

\- Only the first bounding box of each record is used.

\- Map sheets that cross the state border are often classified as "near", because their center lies just outside the outline.

\- The rules (center inside the outline, more than 50 % inside the rectangle) are choices and can be changed.

\- The check shows where the metadata says a dataset is. It flags suspicious boxes, but it cannot prove that a box is wrong.



\## Tools



Python · GeoPandas · Shapely · NumPy · Folium



\## License



MIT, see \[LICENSE](LICENSE).



