.. _samlePlateSurvey:

.. index::
    Sample Plate Survey

Sample Plate Survey
===================

Most users of our instrument have many samples which need to be measured. Over time we have developed many sample holders, which can hold relatively significant number of samples - up to 100 at this time. Using these sample holders makes experiments lot more efficient, convenient and suitable for remote experiments of mail in experiments. The tool described here is part of Indra package from August 1, 2020. *You therefore need version which was released after August 1, 2020*. If there is normal or beta release after this date, use that. Until release is available, install latest Master version (instructions are in the movie or in the help in installer).

*Purpose* This tool enables users to prefill list of samples in the table and send it to instrument ahead of their measurements. The same tool can be used at the instrument to fine tune measurement positions using radiography and export the command file. This way user avoids having to type names and positions while there is beam. User can type these positions while mounting the samples on these plates, making the process less error prone and more efficient.

*Start the tool* To start the tool, select **Setup Sample Plates** from USAXS Menu.

.. Figure:: media/SamplePlate1.jpg
           :align: left
           :width: 430px
           :figwidth: 450px

**General description**: The panel is composed of four main parts: Top selection area, two tabs in the middle and bottom area. Controls in each of these areas are described below. Critical part to understand is, that the purpose of this tool is to fill the table in the Tab0 called "Sample Table" with names of sample, their sx and sy positions and thicknesses. All of this tool is designated to make that easy, convenient and reliable. At the beamline this tool can be used also for survey of sample positions, tweaking and fine tuning where the data will be collected and then exporting the command file. It is further important to understand, that user can create multiple Position sets which can be saved with user selected names. These can be restored in table changed and modified and saved or exported as command file. One Igor experiment can therefore contain many Sample sets. We expect typical user to receive multiple sample plates from us, use this tool to create tables for each while mounting the samples and then provide the Igor experiment to the beamline for either mail-in or remote experiment.

*******

**Top selection controls**

*Create New Sample Set* will create a new empty table in the "Sample Table" tab.

*Add Sample Positions* will append more lines to the table. Both of these buttons use the "Lines =" set variable. Default value is 20, user can change the number as needed.

*Templates* If user is using a template for sample plate we designed and pre-programmed in the tool, like our Acrylic plate which has 9x9 samples, user can populate the table with predefined positions for this plate. Number of our common plates are predefined. Optionally, user can choose "Generic grid holder" which through dialog asks for starting sx/sy position, step in sx and step in sy and number of positions vertically and horizontally. User can therefore create rectangular grid of positions quickly.
User can also create image of the plate using *Create Image* button, which will create scaled version of the sample plate and provide some cool features. See later *Images* for more functionality description.

*Select Saved set* if user saved set of positions (filled table) using the button *Save Positions Set* (at the bottom part), a sample set will be stored in this Igor experiment. Using this popup menu user can select this saved positions set and using button *Load saved Position Set* can restore the positions in the table. Existing set of positions is overwritten, so save your positions first under suitable name, if you do not want to loose those. There is no undo here.

*Beamline survey* this button opens special tool for survey of positions at the beamline. This tool has no functionality except when at the beamline.

*Current set name* This is name for the current set of positions. Random name is generated when buttons are used. User should change this name into meaningful name related to the sample plates they are using. It REALLY helps if  it is easy to identify for anyone - sample plates may have numbers, so use "AcrylicPlate5" or anything sensible.

**Example** Assume that user gets two Acrylic plates and wants to populate the table and fill it up. In this case the following steps are all needed, to generate table with positions for each of the sample positions and also image of the plate to guide in sample mounting.
  1.  Start the tool.
  2.  Select the right plate in template (9x9 Acrylic/magnetic plate" is default)
  3.  Push button *Populate table* (needed lines will be added automatically)
  4.  Push button *Create Image*

.. Figure:: media/SamplePlate2.jpg
           :align: left
           :width: 830px
           :figwidth: 850px

Result is table filled with center positions for openings for the samples. They are indexed, in millimeters, with respect to top right corner, which is defined as sx=0 and sy=0. First two openings are designated for beamline use. Others are for users to use. The red marker in the image shows position of the currently selected row of samples in the table. See later *Images* for more functionality description.

******

**Table of positions** This is table in the first Tab in the middle. Here user needs to fill the important details needed by USAXS instrument to collect data. There are four basic data we need:
  1.  *Sample name = First column*. This must be name on all systems we use (Linux, Windows, Mac). In order to make things reliable, names must be single word, start with letter, and use only letters, numbers and "_". And be less than 40 characters. System will fix string user inputs in this field to match these requirements. If you do not like the result, make better choices on the sample name yourself.
  2.  sx position. Horizontal distance of measurement position, in millimeters, from defined sx=0 position. Typically from left edge of the holder, but is kind of arbitrary and can be anything.
  3.  sy position. Vertical distance of measurement position, in millimeters, from defined sy=0 position. Typically from top edge of the holder, but is kind of arbitrary and can be anything.
  4. Sample thickness, in millimeters. Needed to put data on absolute intensty scale. If not filled by user, Tab "Option Control" has default value which will be used. Can be 0 for blanks.

*Important note* - any line with no name in it is skipped as empty line when creating command file. Fill only used positions with sample names and you can leave the other lines in there. Or delete. See later.

*Right click menu* on the *Table of positions* provide lots of useful functionality:

.. Figure:: media/SamplePlate3.jpg
           :align: left
           :width: 430px
           :figwidth: 450px

*Insert new line* inserts one row at the selected row, moving the rest lower.

*Delete selected rows* delete selected row, rest moves up.

*Duplicate selected row* duplicates the values in selected row and inserts a new row filled with these values in the table. Useful when you need to measure sample twice in positions close together. Duplicate line, change sx or sy (or both) and done.

*Set line as Blank* writes in Sample name string "Blank"

*Write same name to all empty* asks for string and inputs this string into all empty fields. useful when all samples have same prefix and user needs to just append index or code.

*Same Sx to all empty* fills all empty values for sx in the table with value which user is asked for.

*Same Sy to all empty* fills all empty values for sy in the table with value which user is asked for.

*Increment sx from first* Takes value for sx in the first row, asks user for step and inserts increasing sx values to all other rows. Great if user needs to step through the same at fixed distances.

*Increment sy from first* Takes value for sy in the first row, asks user for step and inserts increasing sy values to all other rows. Great if user needs to step through the same at fixed distances.

.. Figure:: media/SamplePlate4.jpg
           :align: left
           :width: 430px
           :figwidth: 450px

**Option Controls** In this tab user can select various options. The most common one will be options "USAXS All?", "SAXS all?", and "WAXS all?". When selected, all samples in the table are measured using that technique. If user does not need one or two of those techniques, uncheck the measurement and that segment will be skipped.
*Default sample thickness* can be set for set of samples (e.g., NMR tubes are 4mm ID) and then thickness does not have to be provided in the table.
*Default Command file name* - do not change unless you really know what you are doing. Name of macro file being exported.
*GUI Controls* rarely needed, "Display individual controls" will enable user to choose - per sample - when to run which measurement segment. Basically, bad idea unless you know why you need it. Talk to staff, but "DO NOT DO IT".
"Display all samples in image" will show red dost and names in the image for all samples in the table. useful when looking for open space in mostly filled table.

*******

**Bottom controls** There are few buttons in this area. This is what gets run when user finishes with the top parts.
*Save Position Set* - will save - inside this Igor experiment - the position set in the table. Name is at the top of the table.
*Preview cmd file* will create Igor notebook with the commands for inspection.
*Export cmd file* will save the command file, as text, with name in the "Default command file name" field (usaxs.mac is strongly suggested) on your *Desktop*.
*Dialog Export cmd file* will save the command file through save-as dialog, so user can pick any location on user computer.


*******

**Images** Images of sample plates provide multiple functionality for users.
  1.  They serve as visual guidance when mounting the samples. Pick row in which you want to place sample and red marker will show position on the plate. The purpose is to minimize mistakes.
  2.  User can right click on position in the image and select one of two right click menu options.
    a.  "Write position" - this will write sx and sy for the position of the click into the currently selected row in the table.
    b.  "Append line with position" this will append a new line at the end of the table with the sx and sy positions of the right click.
  3.  Note that the image is in real units and can be zoomed in and out without loss of functionality.
Images may not exist for all plates beamline has. Future functionality which is not implemented yet is, that user will be able to take a picture of the plate, import in the tool, define the four corners and provide their coordinates in sx and sy units. Then this image will provide same functionality as the defined plates. But this is under development and may not work yet.

******

**Survey at the beamline**

At the beamline the button *Beamline survey* will open a new panel. This panel can control the instrument and should be used with help of radiography to fine tune measurement positions.

.. Figure:: media/SamplePlate5.jpg
           :align: left
           :width: 430px
           :figwidth: 450px


*The top part* are numbers related to row selection in the *Table of positions*. In the figure Table of positions in the main panel has selected row 3 (rows numbering is zero based, the first one is row=0). The buttons "Row down" and "Row up" let user move between rows. Another option is to click on the table and select different row. This tool will sync. Note, than when there is no more rows at the end of the table, new rows will be added when button "Row down" is pushed.

*Sa Name* and *Sa Thickness* are names and thickness from the table. User can edit them here and when button "Save Values" is pushed, these are copied into the table in the selected row.

*Sa X tbl* and *Sa Y Tbl* are sx and sy values from table.

*Drive to table values* button will move sx and sy to the Table values (above). Will work ONLY if both sx and sy have meaningful numbers in, if any is empty, no motion is done.  Note, that this code will refuse to move sx and sy while instrument is collecting data.

*Drive to SX/SY on row change?* when this checkbox is selected, when user changes row, the code will move to sx and sy positions from that row, if possible.

*Save values* button will save current sx and sy motor positions in selected row. It will also copy in that row sample name and thickness.


*Bottom part* these are simply motor controls, similar to our motor GUI. There is SX and SY values - motor positions read from epics. Left right arrows will make step by the step value from the current position. Epics is read about 10x second.

*Step controls* Steps can be controlled three different way. Arrows up/down next to the value change step by 1mm up or down. Buttons "x 0.1" make step 10x smaller and buttons "x 10" will make the step 10x larger.
