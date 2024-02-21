# utl-scraping-san-francisco-hourly-weather-forecast-reading-xml-on-the-web
Scraping San Francisco hourly weather forecasts from the web reading xml on the web
    %let pgm=utl-scraping-san-francisco-hourly-weather-forecast-reading-xml-on-the-web;

    Scraping San Francisco hourly weather forecasts from the web reading xml on the web

    github
     http://tinyurl.com/5y4ee46k
     https://github.com/rogerjdeangelis/utl-scraping-san-francisco-hourly-weather-forecast-reading-xml-on-the-web

    related repos on end

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   INPUT                                                                                                                */
    /*   =====                                                                                                                */
    /*                                                                                                                        */
    /*   YOU ONLY NEED THE URL                                                                                                */
    /*                                                                                                                        */
    /*   Web Link                                                                                                             */
    /*   http://tinyurl.com/ynkp9cte                                                                                          */
    /*   http://forecast.weather.gov/MapClick.php?lat=29.803&lon=-82.411&FcstType=digitalDWML                                 */
    /*                                                                                                                        */
    /*   WHAT THE INPUT LOOKS LIKE ON THE WEB (There are other weather conditions available, ie precititation)                */
    /*                                                                                                                        */
    /*   <data>                                                                                                               */
    /*   <location>                                                                                                           */
    /*   <location-key>point1</location-key>                                                                                  */
    /*   <point latitude="29.81" longitude="-82.42"/>                                                                         */
    /*   <area-description>3 Miles S La Crosse FL</area-description>                                                          */
    /*   <height datum="mean sea level" height-units="feet">148</height>                                                      */
    /*   </location>                                                                                                          */
    /*   <moreWeatherInformation applicable-location="point1">                                                                */
    /*   http://forecast.weather.gov/MapClick.php?lat=29.81&lon=-82.42&FcstType=digital                                       */
    /*   </moreWeatherInformation>                                                                                            */
    /*   <time-layout time-coordinate="local" summarization="none">                                                           */
    /*   <layout-key>k-p1h-n1-0</layout-key>                                                                                  */
    /*   <start-valid-time>2017-06-13T09:00:00-04:00</start-valid-time>                                                       */
    /*   <end-valid-time>2017-06-13T10:00:00-04:00</end-valid-time>                                                           */
    /*   <start-valid-time>2017-06-13T10:00:00-04:00</start-valid-time>                                                       */
    /*   <end-valid-time>2017-06-13T11:00:00-04:00</end-valid-time>                                                           */
    /*   <start-valid-time>2017-06-13T11:00:00-04:00</start-valid-time>                                                       */
    /*   <end-valid-time>2017-06-13T12:00:00-04:00</end-valid-time>                                                           */
    /*   <start-valid-time>2017-06-13T12:00:00-04:00</start-valid-time>                                                       */
    /*   ...                                                                                                                  */
    /*   <weather-conditions>                                                                                                 */
    /*   <value weather-type="rain" coverage="chance"/>                                                                       */
    /*   <value additive="and" weather-type="thunderstorms" coverage="chance"/>                                               */
    /*   </weather-conditions>                                                                                                */
    /*   </weather>                                                                                                           */
    /*   </parameters>                                                                                                        */
    /*   </data>                                                                                                              */
    /*   </dwml>                                                                                                              */
    /*                                                                                                                        */
    /*   PROCESS                                                                                                              */
    /*   =======                                                                                                              */
    /*                                                                                                                        */
    /*   data <- read_xml("http://tinyurl.com/ynkp9cte");                                                                     */
    /*   point <- data %>% xml_find_all("//point");                                                                           */
    /*   lat<-point %>% xml_attr("latitude") %>% as.numeric();                                                                */
    /*   lon<-point %>% xml_attr("longitude") %>% as.numeric();                                                               */
    /*   tym<-data %>%                                                                                                        */
    /*     xml_find_all("//start-valid-time") %>%                                                                             */
    /*     xml_text();                                                                                                        */
    /*   tmp<-data %>%                                                                                                        */
    /*     xml_find_all("//temperature[@type=''hourly'']/value") %>%                                                          */
    /*     xml_text() %>%                                                                                                     */
    /*     as.integer();                                                                                                      */
    /*   want <- data.frame(                                                                                                  */
    /*       lat=lat,                                                                                                         */
    /*       lon=lon,                                                                                                         */
    /*       tmp=tmp,                                                                                                         */
    /*                                                                                                                        */
    /*   OUTPUT                                                                                                               */
    /*   ======                       HOUR                                                                                    */
    /*                                                                                                                        */
    /*             0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 2 2 2 2                                                            */
    /*             0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3                                                            */
    /*          ---+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+---                                                         */
    /*          |                                                   |                                                         */
    /*          | San Francisco 2024-02-22 Hourly Temperatures      |                                                         */
    /*          |                                                   |                                                         */
    /*          | Looks like a sine curve        73                 |                                                         */
    /*       73 + Fit a sine curve?         73 * * * 73             + 73                                                      */
    /*       71 + Sun follows a sine curve   * 71    * 71           + 71                                                      */
    /*       67 +                          * 67                     + 67                                                      */
    /*    T  66 +                                      * 66         + 66 T                                                    */
    /*    E  63 +                        * 63                       + 63 E                                                    */
    /*    M  59 +                                        * 59       + 59 M                                                    */
    /*    P  57 +                      * 57                         + 57 P                                                    */
    /*    E  56 +                                          * 56     + 56 E                                                    */
    /*    T  53 +                                            * 53   + 53 T                                                    */
    /*    U  51 +                                              * 51 + 51 U                                                    */
    /*    R  50 +                                             50 *  + 50 R                                                    */
    /*    E  48 +                    * 48 Temperatures              + 48 E                                                    */
    /*       42 +  * 42                                             + 42                                                      */
    /*       41 +    * 41                                           + 41                                                      */
    /*       40 +                  * 40                             + 40                                                      */
    /*       39 +      * 39                                         + 39                                                      */
    /*       38 +        * 38                                       + 38                                                      */
    /*       37 +          * 37 36                                  + 37                                                      */
    /*       36 +         36 * * * 36                               + 36                                                      */
    /*          |                                                   |                                                         */
    /*          ---+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+---                                                         */
    /*             0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 2 2 2 2                                                            */
    /*             0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3                                                            */
    /*                                                                                                                        */
    /*                                  HOUR                                                                                  */
    /*                                                                                                                        */
    /*          LAT       LON        DATE       TIME     TEMP    HOUR                                                         */
    /*                                                                                                                        */
    /*         29.81    -82.42    2024-02-22    00:00     42      00                                                          */
    /*         29.81    -82.42    2024-02-22    01:00     41      01                                                          */
    /*         29.81    -82.42    2024-02-22    02:00     39      02                                                          */
    /*         29.81    -82.42    2024-02-22    03:00     38      03                                                          */
    /*         29.81    -82.42    2024-02-22    04:00     37      04                                                          */
    /*         29.81    -82.42    2024-02-22    05:00     36      05                                                          */
    /*         29.81    -82.42    2024-02-22    06:00     36      06                                                          */
    /*         29.81    -82.42    2024-02-22    07:00     36      07                                                          */
    /*         29.81    -82.42    2024-02-22    08:00     40      08                                                          */
    /*         29.81    -82.42    2024-02-22    09:00     48      09                                                          */
    /*         29.81    -82.42    2024-02-22    10:00     57      10                                                          */
    /*         29.81    -82.42    2024-02-22    11:00     63      11                                                          */
    /*         29.81    -82.42    2024-02-22    12:00     67      12                                                          */
    /*         29.81    -82.42    2024-02-22    13:00     71      13                                                          */
    /*         29.81    -82.42    2024-02-22    14:00     73      14                                                          */
    /*         29.81    -82.42    2024-02-22    15:00     73      15                                                          */
    /*         29.81    -82.42    2024-02-22    16:00     73      16                                                          */
    /*         29.81    -82.42    2024-02-22    17:00     71      17                                                          */
    /*         29.81    -82.42    2024-02-22    18:00     66      18                                                          */
    /*         29.81    -82.42    2024-02-22    19:00     59      19                                                          */
    /*         29.81    -82.42    2024-02-22    20:00     56      20                                                          */
    /*         29.81    -82.42    2024-02-22    21:00     53      21                                                          */
    /*         29.81    -82.42    2024-02-22    22:00     51      22                                                          */
    /*         29.81    -82.42    2024-02-22    23:00     50      23                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    Web Link
    http://tinyurl.com/ynkp9cte
    http://forecast.weather.gov/MapClick.php?lat=29.803&lon=-82.411&FcstType=digitalDWML

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    /*----                                                                   ----*/
    /*---- change backticks to single quotes to run outside dropdown         ----*/
    /*----                                                                   ----*/

    %utlfkil(d:/xpt/want.xpt);

    %utl_submit_r64x('
    library(xml2);
    library(dplyr);
    library(SASxport);
    data <- read_xml("http://tinyurl.com/ynkp9cte");
    point <- data %>% xml_find_all("//point");
    lat<-point %>% xml_attr("latitude") %>% as.numeric();
    lon<-point %>% xml_attr("longitude") %>% as.numeric();
    tym<-data %>%
      xml_find_all("//start-valid-time") %>%
      xml_text();
    tmp<-data %>%
      xml_find_all("//temperature[@type=''hourly'']/value") %>%
      xml_text() %>%
      as.integer();
    want <- data.frame(
        lat=lat,
        lon=lon,
        tmp=tmp,
        tym=tym);
    want;
    for (i in seq_along(want)) {
              label(want[,i])<- colnames(want)[i]
           };
    write.xport(want,file="d:/xpt/want.xpt")
    ');

    proc datasets lib=work mt=data mt=view; delete want want_r_long_names; run;quit;

    /*--- handles long variable names by using the label to rename the variables  ----*/

    libname xpt xport "d:/xpt/want.xpt";
    proc contents data=xpt._all_;
    run;quit;

    data want_py_long_names;
      %utl_rens(xpt.want) ;
      set want;
    run;quit;
    libname xpt clear;

    proc print data=want_py_long_names;
    run;quit;

    data want_sas_datetime;
      retain lat lon date time temp;
      set want_py_long_names(rename=tmp=temp where=(tym=:'2024-02-22'));
      date=substr(tym,1,10);
      time=substr(tym,12,5);
      hour=substr(time,1,2);
      drop tym;
    run;quit;

    options ls=64 ps=32;
    proc plot data=want_sas_datetime;
     plot temp*hour='*' $ temp  /box;
    run;quit;

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*                                HOUR                                                                                    */
    /*                                                                                                                        */
    /*             0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 2 2 2 2                                                            */
    /*             0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3                                                            */
    /*          ---+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+---                                                         */
    /*          |                                                   |                                                         */
    /*          | San Francisco 2024-02-22 Hourly Temperatures      |                                                         */
    /*          |                                                   |                                                         */
    /*          | Looks like a sine curve        73                 |                                                         */
    /*       73 + Fit a sine curve?         73 * * * 73             + 73                                                      */
    /*       71 +                            * 71    * 71           + 71                                                      */
    /*       67 +                          * 67                     + 67                                                      */
    /*    T  66 +                                      * 66         + 66 T                                                    */
    /*    E  63 +                        * 63                       + 63 E                                                    */
    /*    M  59 +                                        * 59       + 59 M                                                    */
    /*    P  57 +                      * 57                         + 57 P                                                    */
    /*    E  56 +                                          * 56     + 56 E                                                    */
    /*    T  53 +                                            * 53   + 53 T                                                    */
    /*    U  51 +                                              * 51 + 51 U                                                    */
    /*    R  50 +                                             50 *  + 50 R                                                    */
    /*    E  48 +                    * 48                           + 48 E                                                    */
    /*       42 +  * 42                                             + 42                                                      */
    /*       41 +    * 41                                           + 41                                                      */
    /*       40 +                  * 40                             + 40                                                      */
    /*       39 +      * 39                                         + 39                                                      */
    /*       38 +        * 38                                       + 38                                                      */
    /*       37 +          * 37 36                                  + 37                                                      */
    /*       36 +         36 * * * 36                               + 36                                                      */
    /*          |                                                   |                                                         */
    /*          ---+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+---                                                         */
    /*             0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 2 2 2 2                                                            */
    /*             0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3                                                            */
    /*                                                                                                                        */
    /*                                  HOUR                                                                                  */
    /*                                                                                                                        */
    /*          LAT       LON        DATE       TIME     TEMP    HOUR                                                         */
    /*                                                                                                                        */
    /*         29.81    -82.42    2024-02-22    00:00     42      00                                                          */
    /*         29.81    -82.42    2024-02-22    01:00     41      01                                                          */
    /*         29.81    -82.42    2024-02-22    02:00     39      02                                                          */
    /*         29.81    -82.42    2024-02-22    03:00     38      03                                                          */
    /*         29.81    -82.42    2024-02-22    04:00     37      04                                                          */
    /*         29.81    -82.42    2024-02-22    05:00     36      05                                                          */
    /*         29.81    -82.42    2024-02-22    06:00     36      06                                                          */
    /*         29.81    -82.42    2024-02-22    07:00     36      07                                                          */
    /*         29.81    -82.42    2024-02-22    08:00     40      08                                                          */
    /*         29.81    -82.42    2024-02-22    09:00     48      09                                                          */
    /*         29.81    -82.42    2024-02-22    10:00     57      10                                                          */
    /*         29.81    -82.42    2024-02-22    11:00     63      11                                                          */
    /*         29.81    -82.42    2024-02-22    12:00     67      12                                                          */
    /*         29.81    -82.42    2024-02-22    13:00     71      13                                                          */
    /*         29.81    -82.42    2024-02-22    14:00     73      14                                                          */
    /*         29.81    -82.42    2024-02-22    15:00     73      15                                                          */
    /*         29.81    -82.42    2024-02-22    16:00     73      16                                                          */
    /*         29.81    -82.42    2024-02-22    17:00     71      17                                                          */
    /*         29.81    -82.42    2024-02-22    18:00     66      18                                                          */
    /*         29.81    -82.42    2024-02-22    19:00     59      19                                                          */
    /*         29.81    -82.42    2024-02-22    20:00     56      20                                                          */
    /*         29.81    -82.42    2024-02-22    21:00     53      21                                                          */
    /*         29.81    -82.42    2024-02-22    22:00     51      22                                                          */
    /*         29.81    -82.42    2024-02-22    23:00     50      23                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */


    %utl_submit_r64x('
    library(xml2);
    library(dplyr);
    library(SASxport);
    data <- read_xml("http://forecast.weather.gov/MapClick.php?lat=29.803&lon=-82.411&FcstType=digitalDWML");
    point <- data %>% xml_find_all("//point");
    lat<-point %>% xml_attr("latitude") %>% as.numeric();
    lon<-point %>% xml_attr("longitude") %>% as.numeric();
    tym<-data %>%
      xml_find_all("//start-valid-time") %>%
      xml_text();
    tmp<-data %>%
      xml_find_all("//temperature[@type=''hourly'']/value") %>%
      xml_text() %>%
      as.integer();
    want <- data.frame(
        lat=lat,
        lon=lon,
        tmp=tmp,
        tym=tym);
    want;
    for (i in seq_along(want)) {
              label(want[,i])<- colnames(want)[i]
           };
    write.xport(want,file="d:/xpt/want.xpt")
    ');


    REPO
    -----------------------------------------------------------------------------------------------------------------------------------
    https://github.com/rogerjdeangelis/utl-Web-Scraping-data-from-Australian-Open-stats
    https://github.com/rogerjdeangelis/utl-identifying-html-nodes-and-extracting-specific-information-scraping-html
    https://github.com/rogerjdeangelis/utl-identifying-the-html-table-and-exporting-to-spss-then-sas-scraping
    https://github.com/rogerjdeangelis/utl-locating-the-html-node-and-web-scrape-the-html-table
    https://github.com/rogerjdeangelis/utl-scraping-a-single-indirect-html-reference-using-python-beautiful-soup-and-request-packages
    https://github.com/rogerjdeangelis/utl-scraping-pdf-output-for-pdf-tables-and-lists
    https://github.com/rogerjdeangelis/utl-scraping-server-screens-when-Copy-Print-PageSource-are-disabled-python-tesseract
    https://github.com/rogerjdeangelis/utl-web-scaping-Loop-over-many-URLs-scrape-parse-extract-nodes-and-put-into-a-sas-table
    https://github.com/rogerjdeangelis/utl-web-scraping-USDOT-vehicle-inspections-and-fatalities
    https://github.com/rogerjdeangelis/utl-web-scraping-a-large-wikipedia-html-table-user-R-rvest
    https://github.com/rogerjdeangelis/utl-web-scraping-spains-grananda-soccer-team-players
    https://github.com/rogerjdeangelis/utl_scrape_javascript_converting_table_to_sas_dataset_no_browser
    https://github.com/rogerjdeangelis/utl_scraping_javascript_generated_html_web_pages
    https://github.com/rogerjdeangelis/utl_scraping_mutiple_web_tables_and_creating_seven_database_tables
    https://github.com/rogerjdeangelis/utl_web_scrape_23_separate_pages_one_per_state_for_all_whole_food_store_addresses
    https://github.com/rogerjdeangelis/utl_web_scraping_programatically_using_a_web_search_box_and_retrieve_information
    https://github.com/rogerjdeangelis/utl_web_scraping_top_cnn_stories
    https://github.com/rogerjdeangelis/utl_web_scraping_web_pages_for_the_latest_news_on_the_food_shortage_in_yemen
    https://github.com/rogerjdeangelis/utl_webscraping_www.treasury.gov__xml_page_of_interest_rates

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
