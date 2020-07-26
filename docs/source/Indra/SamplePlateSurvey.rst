.. _samlePlateSurvey:

.. index::
    Sample Plate Survey

Sample Plate Setup tool
=======================

Most users of our instrument have many samples which need to be measured. Over time we have developed various sample holders, which can hold up to 100 (at this time) of samples at once. Using these sample holders makes experiments lot more efficient, convenient, and suitable for remote experiments or mail-in experiments. The *Sample Plate Setup* tool described here is part of Indra (USAXS) package from August 1, 2020. *You therefore need version which was released after August 1, 2020*. If there is normal or at least beta version released after 8/1/2020, use that release. Until new release is available, install the latest Master version (instructions are in the movie or in the help in installer).

*Purpose*

This tool enables users to pre fill one or more "Sample table" and send it to instrument ahead of their measurements. The same tool can be used at the beamline to fine tune measurement positions using radiography and export the command file. This way user avoids having to type sample names and positions at the beamline, while there is beam. User can type these positions while mounting the samples on these plates, making the process less error prone and more time efficient.

How to plan experiment (and sample plate)
-----------------------------------------

Which sample plate (aka "sample holder") is needed depends on sample type (solid, powder, liquid), how large the samples are, what conditions need to be kept during shipping, measurement, and storage and also, how fragile the sample is. Users should talk with staff and based on the discussion and information provided on this page (https://usaxs.xray.aps.anl.gov/documentation/sample-environments) pick the right sample plate/holder. Note, that sample plates are being developed or changed all the time and therefore that page is updated routinely. Check again, even if you have used instrument before.

Let now us assume, that you have picked suitable sample plate/holder from the list and have beamtime scheduled few weeks from now. You should contact the staff at the instrument (mailto:usaxs@aps.anl.gov) at least 3 weeks before your experiment start and request suitable number of specific plates to be shipped to you. Alternative is to use drawings provided on above linked sample environments web page and 3D print your own or get one milled in machine shop. Neither is too expensive today. these are simple structures. At least two weeks before your experiment you should *receive* needed number of sample plates of suitable types.

Now you need to plan data collection. Here are some rules to keep in mind:
  * Data collection for one sample (or Blank) is about 4 minutes (typical) with USAXS about 120 seconds, SAXS and WAXS about 20-30 seconds each. Rest is moving instrument around, transmission measurement etc.
  * There is overhead with survey, calibration and radiography, so typically you need 5-6 minutes of overall time per each sample. That is ~10 samples/hour, up to ~80 samples/shift. Moving samples around takes time, minimize the motions.
  * You need to measure suitable instrumental curve - "Blank" - one for each about 5-10 sample measurements. Blank is everything which is in the beam EXCEPT your sample. If you have powder in Scotch tape, Blank is Scotch tape pouch without the powder. If you have suspension in water, Blank is water with capillary/NMR tube without the suspended material, etc. Talk to staff if not clear. *It is important*
  * You need to measure Blanks throughout the data collection - not all at the beginning or at the end. Instrument background may evolve during time, so you need to measure Blank - few samples - Blank - few samples - Blank etc.
  * Blank measurement is critically important. Without suitable Blank no useful data can be obtained from the USAXS instrument. Background is significant so it MUST be subtracted - and normalization/calibration cannot be done either.
  * Sample Thickness is important. Data from USAXS instrument are on absolute intensity scale if correct sample thickness is provided. This does not apply for samples which cannot/will not be put on absolute intensity scale (powders,..). In that case set default thickness to 1mm and problem solved. Thickness is in millimeters (it is converted correctly later in data reduction).

Mount the samples on the plates. If there is pre defined Template for the one you are using (check below), start with Template for this plate. It will make your life easier. Horizontal direction, when mounted in the instrument, is called SX and vertical is SY. Some obvious feature on each plate is designated SX=0 and SY=0 (typically top right corner) and all distances are measured from this position, in millimeters. Position designation is arbitrary and you are welcome to designate for your own holders (if staff approves their use ahead) any feature which is easy to find.

While you are mounting the samples, keep in mind, that the samples need to be shipped to us. They need to stay mounted during shipping. Tape the samples sufficiently well. Mark the samples with names so if they fall off, staff can remount them. Scotch Magic tape seems to be preferred tape based on our experience, as it has relatively low background signal and no diffraction peaks (Kapton has diffraction peaks in SAXS region). Talk to staff.

Fill the table in the tool while you mount the samples - or after you mounted all the samples. How to do it depends on your preferences. Here are some obvious options:
  * Fill the table with names and positions manually. Create suitable number of lines in Sample Table and start typing.
  * If image of the sample plate is available, fill names for all samples in the table (do not forget to add Blanks) and then select a row in the table and right click in the image on place where the sample is located and write the values there.
  * At the beamline, where "Beamline survey" works, you can be adding lines with new samples using the "Beamline survey tool".
  * If all samples have same thickness (or thickness is unknown), set default thickness in the "Options control" and leave the Thickness column empty. If sample thickness varies, write thickness to those which do not have default thickness, leave empty for those which do.
  * Sample Names need to be suitable name for our instrument - string, starting with letter, no spaces, only letters, numbers, and "_". When a name is written in the table column "Sample name", the name is checked and if needed, converted in suitable string. If you do not like what you see, make different choice for name.

Verify positions and names. If plate image is available, use the image to show you where sample for each row is.



*SAVE THE POSITION SET & IGOR EXPERIMENT*

Change the default (and *meaningless*) unique name into name which matches somehow the sample plate and can be easily identified. If you have Plate#5 and Plate#6 from us, save positions for Plate#5 as "Plate_5" etc. Keep it simple! *It may be 2am when staff is trying to understand this.* Save the Igor experiment with meaningful name, preferably containing your own name or GUP number or whatever is easy to understand. Unique name. Sensible name. For mail-in e-mail Igor experiment to staff. For remote access you can use NoMachine file transfer and push it from your computer to control computer at the beamline and/or e-mail the Igor experiment to staff ahead of your experiment. *DO NOT e-mail the command files* commend files will be generated at the beamline after checking the sample positions with radiography.

Here are few more of practical suggestions for shipping and safety:
  * Samples can fall off, make sure they are marked with names and can be identified.
  * Keep the sample locations as simple as possible.
  * Package suitably for shipping. Bubble wrap if reasonable.
  * Ship with at least two days to spare.
  * There is no weekend delivery at ANL, samples for weekend must arrive latest Thursday morning.
  * Talk to staff about shipping.
  * *Send staff necessary safety and chemistry information.*
  * For remote operations, submit ESAF at least 14 days before your experiment. Mail-in ESAF is handled by staff, they need chemistry and safety information well ahead of your measurement.


Tool Description
----------------

*Start the tool* To start the tool, select **Setup Sample Plates** from USAXS Menu.

.. Figure:: media/SamplePlate1.jpg
           :align: left
           :width: 450px
           :figwidth: 470px

**General description**: The panel is divided into four main parts:
 * Top Controls
 * Tab with Sample Table
 * Tab with Options control
 * Bottom area with save and output buttons.
 * And message which reports to user last action he/she did!

Controls in each of these areas are described below. The main purpose of this tool is to help users fill the "Sample Table" with Sample names, sx, sy, thickness and in the future metadata. This tool should make that easy, convenient, and reliable. *At the beamline* this tool can also be used for survey of sample positions, tweaking and fine tuning sx and sy for measurements and creating the command file.

It is important to understand, that user can create multiple *Position sets* which can be stored with user selected names inside *One Igor Experiment*. These Position sets can be restored into the Sample table, changed, and saved or exported as command file. One Igor experiment can therefore contain many Sample sets. We expect typical user to have multiple sample plates, use this tool to create Position set for each sample plates (while mounting the samples) and then deliver to us one Igor experiment for their mail-in or remote experiment.

*******

**Top selection controls**

*Create New Sample Set* will create a new empty table in the "Sample Table" .

*Add Sample Positions* will append more lines to the end of the "Sample Table". Both of these buttons use the "Lines =" value to decide, how many lines are created. Default value is 20, user can change the number as needed.

*Templates* If user is using a standard sample plate we designed and pre-programmed in the tool, like our Acrylic plate which has 9x9 samples, user can populate the table with predefined positions for this plate. Number of our common plates are predefined, more will be added over time. Optionally, user can choose "Generic grid holder" which through dialog asks for starting sx/sy position, step in sx and step in sy and number of positions vertically and horizontally. User can therefore create rectangular grid of positions quickly.
User can also create image of the plate using *Create Image* button, which will create scaled version of the sample plate and provide some cool features. See later *Images* for more functionality description.

*Select Saved set* If user saved a "Set of positions" (= filled table) using the button *Save Positions Set* (at the bottom of this panel), a sample set will be stored in this Igor experiment. Using this popup menu, user can select this saved positions set and using button *Load saved Position Set* can restore the positions in the table. Existing set of positions is overwritten, so save your positions first under suitable name, if you do not want to loose those. There is no undo here.

*Beamline survey* this button opens special tool for survey of positions at the beamline. This tool will open only at the beamline. It is described at the bottom of this help page, if needed.

*Current set name* This is name for the current set of positions. Random name is generated when buttons are used. User should change this name into meaningful name related to the sample plates they are using. It REALLY helps if  it is easy to identify for anyone - sample plates may have numbers, so use "AcrylicPlate5" or anything sensible.

-----

**Example** Assume that user you have received two Acrylic plates and want to populate a table for each and fill in sample names and positions. The following steps are needed to generate table with positions and display image of the plate to guide in sample mounting.
  1.  Start the tool.
  2.  Select the correct *Template*  (e.g., 9x9 Acrylic/magnetic plate" which is default)
  3.  Push button *Populate table* (needed lines will be added automatically)
  4.  Push button *Create Image*

.. Figure:: media/SamplePlate2.jpg
           :align: left
           :width: 830px
           :figwidth: 850px

Result is table, pre filled with center positions for each sample position. Positions are indexed, in millimeters, with respect to top right corner, which is defined as sx=0 and sy=0. First two openings are designated for beamline use. Others are for users to use. The red marker in the image shows position of the currently selected row of samples in the table. See later *Images* for more functionality description. Fill the table for Plate 1, mounting up to 79 samples on this plate.
  5. *Set Name* for the plate into easy to identify name which is clearly related to the plate in front of your (e.g., Plate5 if the Plate has sticker "Plate#5").
  6. *Save Position Set* using the button in the bottom left corner.
  7. Use steps 3-6 to create a table for second plate and populate it with sample names/positions.
  8. Save Igor Experiment with meaningful name (e.g. "MyName_USAXS_20200805.pxp"). Send this experiment to staff. Following other instructions at the top of this page ship the plates with samples mounted to the instrument.

******

**Sample Table**

Here user needs to fill the important details needed by USAXS/SAXS/WAXS instrument to collect data. There are four basic values we need:
  1.  *Sample name = First column*. This must be acceptable filename on all systems we use (Linux, Windows, Mac). In order to make things reliable, names must be single word, start with letter, and use only letters, numbers, and "_". And be less than 40 characters long. System will fix user input in this field to match these requirements. If you do not like the result, edit it - but make better choices on your sample name. User name passing the above requirements will not be modified.
  2.  SX position. This is horizontal distance of measurement position, in millimeters, from defined sx=0. Typically from right edge of the holder, but is kind of arbitrary and can be any location.
  3.  SY position. This is vertical distance of measurement position, in millimeters, from defined sy=0. Typically from the top edge of the holder, but is kind of arbitrary and can be anything.
  4. Sample thickness, in millimeters. Needed to put data on absolute intensity scale. If not filled by user, "Option Control" has default value which will be used. Can be 0 for blanks.

*Important note* - any line with no Sample name in it is considered empty line and will be skipped when creating command file. Fill Sample Names only for used positions and you can leave the other lines in there. Or delete. See later.

*Right click menu* on the *Sample Table* provide lots of useful functionality:

.. Figure:: media/SamplePlate3.jpg
           :align: left
           :width: 480px
           :figwidth: 500px

*Insert new line*    Inserts one row in the selected row, moving the rest down.

*Delete selected line*    Deletes selected row, rest moves up.

*Duplicate selected line*    Inserts a new row in the Sample Table. The new row is filled with values from the row which is being duplicated. Useful when you need to measure sample twice in positions close together. Duplicate line, change sx and/or sy and done.

*Set line as Blank*    Writes in Sample name string Blank

*Write same name to all empty*    Asks for string and inputs this string into all empty Sample Name fields. Useful when all samples have same prefix and user needs to just append index or code.

*Same Sx to all empty*    Asks user for sx value and this one is filled in all empty sx fields in the table. SX fields which contain any number are not changed.

*Same Sy to all empty*    Asks user for sy value and this one is filled in all empty sy fields in the table. SY fields which contain any number are not changed.

*Increment sx from selected row*    Takes value for sx in the selected row, asks user for step and inserts incremented sx values to all higher rows. Step can be negative. Great if user needs to step through the sample at fixed distances.

*Increment sy from selected row*    Takes value for sy in the selected row, asks user for step and inserts incremented sy values to all higher rows. Step can be negative. Great if user needs to step through the sample at fixed distances.

*Copy row values to Table Clipboard*    Copies values in selected row into string ("Table Clipboard") and saves it for later use. There is only ONE Table Clipboard string available to users, copying new row in Table Clipboard will overwrite existing content.

*Paste Table Clipboard to row*    Pastes the values stored in above "Copy" command into the selected row. Overwrites existing values. Note: Table Clipboard is not emptied by this command, same content can be pasted many times.


In order to create a new row with some content, user has two obvious choices :
  * Duplicate a row, in which case a new row with the same content as the one being duplicated is created. But the new row is next the duplicate row. There is no "Move row" function at this time.
  * Copy row content in Table Clipboard, insert empty row in suitable place and paste the content of Table Clipboard there.

The second method is great if user forgets to insert enough Blank measurements. Copy Blank location, insert number of empty lines throughout the table and paste in those new lines Blank parameters from Table Clipboard.

*******

**Option Controls**


.. Figure:: media/SamplePlate4.jpg
           :align: left
           :width: 380px
           :figwidth: 400px


In this tab user can select various options. The most common one will be options *USAXS All?*, *SAXS all?*, and *WAXS all?*. When selected, all samples in the table are measured using that technique. If user does not need one or two of those techniques, uncheck the measurement and that segment will be skipped.

*Default sample thickness*    Can be set for set of samples (e.g., NMR tubes are 4mm ID) and then thickness does not have to be provided in the Sample table.

*Default Command file name* - do not change unless you really know what you are doing. Name of macro file being exported.

*GUI Controls* are rarely needed.   *Display individual controls*    Will enable user to choose - per sample - when to run which measurement segment. Basically, bad idea unless you know why you need it. Talk to staff, but "DO NOT DO IT".

*Display all samples in image*    Will show red dots and names in the image for all samples in the table. useful when looking for open space in mostly filled table.

**Hook function**

Sometimes we need to modify data collection in way which is rare and difficult to put in GUI. For this purpose we have checkbox *Run Export hook function?*. If this checkbox is checked, code will run for export a "hook" function. This function needs to be modified in the code (or one can use overwrite). Below is example, which for each (non-Blank) position collects actually 5 different positions. Center (defined one) and one up, down, left and right from the center position. This is used to get average over wider area to get larger statistical average.


 Function IN3S_ExportHookFunction(Command, SampleName,SX, SY, Thickness, MD)

	 string Command, SampleName,SX, SY, Thickness, MD

  	//this hook function will modify output of the command file for given line. This needs to be customized for specific need.

	 SVAR nbl=root\:Packages\:SamplePlateSetup\:NotebookName

	 //in this case it will write each command in notebook multiple times, in original position and then +/- 1mm in sx and sy

	 //center

	 Notebook $nbl text="      "+Command+"        "+SX+"      "+SY+"      "+Thickness+"      \""+SampleName+"\"  \r"

	 //and now the variations, only if Sample Name is NOT Blank or Empty

	 if(!StringMatch(SampleName, "*Blank*")&&!StringMatch(SampleName, "*Empty*"))

		 string TempStr

		 TempStr = num2str(str2num(SX)-1)

		 Notebook $nbl text="      "+Command+"        "+TempStr+"      "+SY+"      "+Thickness+"      \""+SampleName+"_R"+"\"  \r"

		 TempStr = num2str(str2num(SX)+1)

		 Notebook $nbl text="      "+Command+"        "+TempStr+"      "+SY+"      "+Thickness+"      \""+SampleName+"_L"+"\"  \r"

		 TempStr = num2str(str2num(SY)-1)

		 Notebook $nbl text="      "+Command+"        "+SX+"      "+TempStr+"      "+Thickness+"      \""+SampleName+"_B"+"\"  \r"

		 TempStr = num2str(str2num(SY)+1)

		 Notebook $nbl text="      "+Command+"        "+SX+"      "+TempStr+"      "+Thickness+"      \""+SampleName+"_T"+"\"  \r"

	 endif

   end



*******

**Bottom controls**

There are few buttons in this area. These are actions run when user finishes setting up the top parts of this panel.

*Save Position Set* - will save - inside this Igor experiment - the position set in the Sample table. Name is at the top of the table.

*Preview cmd file* will create Igor notebook with the commands for inspection.

*Export cmd file* will save the command file, as text, with name in the "Default command file name" field (usaxs.mac is strongly suggested) on your *Desktop*.

*Dialog Export cmd file* will save the command file through save-as dialog, so user can pick any location on user computer and optionally change the name as needed.

*******

**Images**

Images of sample plates provide multiple functionality for users. If they are defined for some Template, user can create such image using button *Create Image*. If they are not defined, user get error message. Images are very helpful, since they serve as visual guidance when mounting the samples. Pick row in which you want to place sample and red marker will show position on the plate. The purpose is to minimize mistakes.

There is right click menu for the image - user can right click (or control/cmd click) on position in the image and select one of two right click menu options.
    a.  *Write position* - this will write sx and sy for the position of the click into the currently selected row in the table.
    b.  *Append line with position* this will append a new line at the end of the table with the sx and sy positions of the right click.

Note that the image is in real millimeters and has grid lines, image can be zoomed in and out without loss of functionality.

Images may not exist for all plates beamline has. Future functionality which is not implemented yet will be that user will be able to take a picture of the plate, import image in the tool. Then user will define the four corners with their sx and sy coordinates, image will be straightened, cropped and displayed in this tool. Such image will provide same functionality as the pre defined plates. But this is under development and may not work yet.

******

**Survey at the beamline**

At the beamline the button *Beamline survey* will open a new panel. This panel can control the instrument and should be used with help of radiography to fine tune measurement positions.

.. Figure:: media/SamplePlate5.jpg
           :align: left
           :width: 330px
           :figwidth: 350px


*The top part* are numbers related to row selected in the *Sample Table*. In the figure Sample Table in the main panel has selected row 3 (rows numbering is zero based, the first one is row=0, so this is actually fourth row). The buttons "Row down" and "Row up" let user move between rows. Another option to move to different row is to select different row in the Sample Table. This tool will sync. Note, than when there is no more rows at the end of the Sample table, a new empty row will be added when button "Row down" is pushed.

*Sa Name* and *Sa Thickness* are Sample name and thickness from the Sample table on the main panel. User can edit them here and when button "Save Values" is pushed, these are copied into the table in the selected row. Sample Name and thickness are both checked for sensibility and cleaned up if necessary.

*Sa X tbl* and *Sa Y Tbl* are sx and sy values from the Sample Table. They are red only values here.

*Drive to table values* button will move *instrument* sx and sy to the Sample Table values (above). **THIS MOVES INSTRUMENT** Will work ONLY if both sx and sy have meaningful numbers in, if any is empty, no motion is done.  Note, that this code will refuse to move sx and sy while instrument is collecting data.

*Drive to SX/SY on row change?* if this checkbox is selected, when user changes row, the code will move to sx and sy positions from that row, if possible. It does not matter if the row is changed by button or by selecting a row in the Sample Table.

*Save values* button will save current sx and sy motor positions in selected row in Sample Table. It will also copy in that row Sample Name and Thickness.

**Bottom part**

these are simply motor controls, similar to our standard epics motor GUI. There is SX and SY values - motor positions read from epics. Arrows will make change motor positions by the step value below. Epics is updated about 10x second by background procedure.

*Step controls* Steps can be changed multiple different ways. User can select the value and type in the field. Arrows up/down next to the step value change step by 1mm up or down. Button "x 0.1" makes the step 10x smaller and button "x 10" makes the step 10x larger.

Beamline survey should be disabled for all installations except at the beamline computers. Even at beamline computers, this tool will not move motors if instrument indicates that it is collecting data. Also, this tool does not know anything about epics limits and any other errors or failures in epics, so if motors do not work properly, check epics. Call staff. **DO NOT GET CREATIVE.**
