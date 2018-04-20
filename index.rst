BeerYAML
========

Purpose
-------

**Why are you using another format since beerxml is widely used ?**

The reason is very simple, `XML <https://en.wikipedia.org/wiki/XML>`_ is very
verbose. Furthermore `beerxml <http://www.beerxml.com/beerxml.htm>`_ format is
a very complete data description containing lots of mandatory keys which can be
irrelevant for your recipe. This two formats have their own purpose and are well
formatted markup languages. The goal of this project is not to replace beerxml
but to make it simpler to use in plain text files.

The main purpose of storing recipes in the `YAML <https://en.wikipedia.org/wiki/YAML>`_
format is to make recipes more human-readable. It should be really simple to
write your own recipes in a simple format and read it without needing to use an
external software.

General
-------

Brewing data will follow the YAML standard as a basis. The format shares the
mandatory fields with the beerxml format, then the format parser should be able
to export the recipe into beerxml format. In addition, the format supports
all kind of optional tags. These tags must be parsed by a program to be
compliant.

Differences
-----------

Unlike beerxml format records, a `VERSION` tag is not required in BeerYAML.
However it is highly recommended for a parser to set this tag to be compatible
with beerxml format.

A style tag is required for `Recipe`_ like in the beerxml format. However this
tag can either be a `string` or a list of values like in beerxml format. See
`Style`_ section for more information.

Recipe record sets are optionnals and should be set empty by the parser when
exporting to xml.

It is possible to define a record set name by its YAML key. Thus

    .. code:: yaml

      mash_steps:
        proteic:
          step_time: 60
          step_temp: 100
          type: Infusion

must be equal to

    .. code:: yaml

      mash_steps:
        mash_step:
          name: proteic
          step_time: 60
          step_temp: 100
          type: Infusion

Recipe
------

+------------+--------------------------------------------------------+
| Data tag   | Description                                            |
+============+========================================================+
| name       | Name of the recipe                                     |
+------------+--------------------------------------------------------+
| type       | May be one of "Extract”, "Partial Mash" or "All Grain" |
+------------+--------------------------------------------------------+
| style      | The style of the beer                                  |
+------------+--------------------------------------------------------+
| brewer     | Name of the brewer                                     |
+------------+--------------------------------------------------------+
| batch_size | Target size of the finished batch                      |
+------------+--------------------------------------------------------+
| boil_size  | Starting size for the main boil of the wort            |
+------------+--------------------------------------------------------+
| boil_time  | The total time to boil the wort                        |
+------------+--------------------------------------------------------+


*An example of minimal recipe*

    .. code:: yaml

      name: Test
      brewer: TROUVERIE Joachim
      type: All Grain
      batch_size: 10.0
      boil_time: 60.0
      boil_size: 15.0
      style: Test


Style
-----

A recipe's style can be stored by only its name, like in the example above, or
defining the following tags.

+-----------------+------------------------------------------------------------+
| Data tag        | Description                                                |
+=================+============================================================+
| name            | Name of the style profile                                  |
+-----------------+------------------------------------------------------------+
| category        | Category that this style belongs to – usually associated   |
|                 | with a group of styles such as "English Ales" or "American |
|                 | Lagers".                                                   |
+-----------------+------------------------------------------------------------+
| category_number | Number or identifier associated with this style category.  |
|                 | For example in the BJCP style guide, the "American Lager"  |
|                 | category has a category number of "1".                     |
+-----------------+------------------------------------------------------------+
| style_letter    | The specific style number or subcategory letter associated |
|                 | with this particular style. For example in the BJCP style  |
|                 | guide, an American Standard Lager would be style letter "A"|
|                 | under the main category.  Letters should be upper case.    |
+-----------------+------------------------------------------------------------+
| style_guide     | The name of the style guide that this particular style or  |
|                 | category belongs to. For example "BJCP" might denote the   |
|                 | BJCP style guide, and "AHA" would be used for the AHA style|
|                 | guide.                                                     |
+-----------------+------------------------------------------------------------+
| type            | May be "Lager", "Ale", "Mead", "Wheat", "Mixed" or "Cider" |
|                 | Defines the type of beverage associated with this category.|
+-----------------+------------------------------------------------------------+
| og_min          | The minimum specific gravity as measured relative to water.|
|                 | For example "1.040" might be a reasonable minimum for a    |
|                 | Pale Ale.                                                  |
+-----------------+------------------------------------------------------------+
| og_max          | The maximum specific gravity as measured relative to water.|
+-----------------+------------------------------------------------------------+
| fg_min          | The minimum final gravity as measured relative to water.   |
+-----------------+------------------------------------------------------------+
| fg_max          | The maximum final gravity as measured relative to water.   |
+-----------------+------------------------------------------------------------+
| ibu_min         | The recommended minimum bitterness for this style as       |
|                 | measured in International Bitterness Units (IBUs)          |
+-----------------+------------------------------------------------------------+
| ibu_max         | The recommended maximum bitterness for this style as       |
|                 | measured in International Bitterness Units (IBUs)          |
+-----------------+------------------------------------------------------------+
| color_min       | The minimum recommended color                              |
+-----------------+------------------------------------------------------------+
| color_max       | The maximum recommended color                              |
+-----------------+------------------------------------------------------------+

*Let's see an example with the previous recipe.*

    .. code:: yaml

      name: Test
      # [...] recipe mandatory keys
      style:
        name: Bohemian Pilsner
        category: European Pale Ale
        categoty_number: 2
        style_letter: A
        style_guide: BJCP
        type: Lager
        og_min: 1.044
        og_max: 1.056
        fg_min: 1.013
        fg_max: 1.017
        ibu_min: 35.0
        ibu_max: 45.0
        color_min: 3.0
        color_max: 5.0

Hops
----

Hops composing the recipe. These keys are stored using a `hops` parent key. As
mentioned in the `Differences`_ section, the hop name
can be defined by its YAML key.

+----------+-----------------------------------------------------------+
| Data tag | Description                                               |
+==========+===========================================================+
| name     | Name of the hop                                           |
+----------+-----------------------------------------------------------+
| alpha    | Percent alpha of hop                                      |
+----------+-----------------------------------------------------------+
| amount   | Weight of the hop used in the recipe                      |
+----------+-----------------------------------------------------------+
| use      | May be "Boil", "Dry Hop", "Mash", "First Wort" or "Aroma" |
+----------+-----------------------------------------------------------+
| time     | The time of use                                           |
+----------+-----------------------------------------------------------+

*Example*

    .. code:: yaml

      name: Test
      # [...] recipe mandatory keys
      hops:
        Cascade:
          alpha: 5.0
          amount: 0.100 Kg
          use: Boil
          time: 60 min

Fermentables
------------

Fermentables composing the recipe including extracts, grains, sugars, honey,
fruits. These keys are stored using a `fermentables` parent key. As mentioned
in the `Differences`_ section, the fermentable name can be defined by its YAML
key.

+----------+-------------------------------------------------------------------+
| Data tag | Description                                                       |
+==========+===================================================================+
| name     | Name of the fermentable                                           |
+----------+-------------------------------------------------------------------+
| type     | May be "Grain", "Sugar", "Extract", "Dry Extract" or "Adjunct".   |
|          | Extract refers to liquid extract.                                 |
+----------+-------------------------------------------------------------------+
| amount   | Extract refers to liquid extract.                                 |
+----------+-------------------------------------------------------------------+
| yield    | Percent dry yield (fine grain) for the grain, or the raw yield by |
|          | weight if this is an extract adjunct or sugar                     |
+----------+-------------------------------------------------------------------+
| color    | The color of the item                                             |
+----------+-------------------------------------------------------------------+

*Example*

    .. code:: yaml

      name: Test
      # [...] recipe mandatory keys
      fermentables:
        Pale 2-row Malt:
          amount: 5.0
          type: Grain
          yield: 73.4
          color: 3.0 EBC
Yeasts
------

The term "yeast" encompasses all yeasts, including dry yeast, liquid yeast and
yeast starters. These keys are stored using a `yeasts` parent key. As mentioned
in the `Differences`_ section, the yeast name can be defined by its YAML key.

+----------+-------------------------------------------------------+
| Data tag | Description                                           |
+==========+=======================================================+
| name     | Name of the yeast                                     |
+----------+-------------------------------------------------------+
| type     | May be "Ale", "Lager", "Wheat", "Wine" or "Champagne" |
+----------+-------------------------------------------------------+
| form     | May be "Liquid", "Dry", "Slant" or "Culture"          |
+----------+-------------------------------------------------------+
| amount   | The amount of yeast                                   |
+----------+-------------------------------------------------------+

*Example*


    .. code:: yaml

      name: Test
      # [...] recipe mandatory keys
      yeasts:
        Ole English Ale Yeast:
          amount: 0.1
          type: Ale
          form: Liquid
Miscs
-----

The term "misc" encompasses all non-fermentable miscellaneous ingredients that
are not hops or yeast and do not significantly change the gravity of the beer.
These keys are stored using a `miscs` parent key. As mentioned in the
`Differences`_ section, the misc name can be defined by its YAML key.

+----------+--------------------------------------------------------------+
| Data tag | Description                                                  |
+==========+==============================================================+
| name     | Name of the misc item                                        |
+----------+--------------------------------------------------------------+
| type     | May be "Spice", "Fining", "Water Agent", "Herb", "Flavor" or |
|          | "Other"                                                      |
+----------+--------------------------------------------------------------+
| use      | May be "Boil", "Mash", "Primary", "Secondary", "Bottling"    |
+----------+--------------------------------------------------------------+
| time     | Amount of time the misc was used                             |
+----------+--------------------------------------------------------------+
| amount   | Amount of item used                                          |
+----------+--------------------------------------------------------------+

*Example*


    .. code:: yaml

      name: Test
      # [...] recipe mandatory keys
      miscs:
        Irish Moss:
          type: Fining
          use: Boil
          time: 15.0
          amount: 0.1

Mash profile
------------

A mash profile is a record used either within a recipe or outside the recipe to
precisely specify the mash method used. These keys are stored using a `mash`
parent key. The record consists of some informational items followed by a
`mash_steps` key.

+------------+-----------------------------------------------------------+
| Data tag   | Description                                               |
+============+===========================================================+
| name       | Name of the mash profile                                  |
+------------+-----------------------------------------------------------+
| grain_temp | The temperature of the grain before adding it to the mash |
+------------+-----------------------------------------------------------+

*Example*

    .. code:: yaml

      name: Test
      # [...] recipe mandatory keys
      mash:
        name: Single Step Infusion, 68 C
        grain_temp: 22°C


Mash steps
----------

A mash step is an internal record used within a mash profile to denote a
separate step in a multi-step mash. A mash step is not intended for use outside
of a `Mash profile`_.

These keys are stored using a `mash_steps` parent key. As mentioned in the
`Differences`_ section, the mash_step name can be defined by its YAML key.

+-----------+---------------------------------------------------------------------+
| Data tag  | Description                                                         |
+===========+=====================================================================+
| name      | Name of the mash step – usually descriptive text such as "Dough In" |
|           | or "Conversion"                                                     |
+-----------+---------------------------------------------------------------------+
| type      | May be "Infusion", "Temperature" or "Decoction" depending on the    |
|           | type of step                                                        |
+-----------+---------------------------------------------------------------------+
| step_temp | The target temperature for this step                                |
+-----------+---------------------------------------------------------------------+
| step_time | The number of minutes to spend at this step                         |
+-----------+---------------------------------------------------------------------+

*Example*

    .. code:: yaml

      name: Test
      # [...] recipe mandatory keys
      mash:
        name: Single Step Infusion, 68 C
        grain_temp: 22°C
        mash_steps:
          Conversion step:
            type: Decoction
            step_temp: 68
            step_time: 90

Waters
------

The term "water" encompasses water profiles. Though not strictly required for
recipes, the water record allows supporting programs to record the water profile
used for brewing a particular batch.
These keys are stored using a `waters` parent key. As mentioned in the
`Differences`_ section, the water name can be defined by its YAML key.

+-------------+---------------------------+
| Data tag    | Description               |
+=============+===========================+
| name        | Name of the water profile |
+-------------+---------------------------+
| amount      | Volume of water           |
+-------------+---------------------------+
| calcium     | The amount of Calcium     |
+-------------+---------------------------+
| bicarbonate | The amount of Bicarbonate |
+-------------+---------------------------+
| sulfate     | The amount of Sulfate     |
+-------------+---------------------------+
| chloride    | The amount of Chloride    |
+-------------+---------------------------+
| sodium      | The amount of Sodium      |
+-------------+---------------------------+
| magnesium   | The amount of Magnesium   |
+-------------+---------------------------+
| ph          | The pH of the water       |
+-------------+---------------------------+

*Example*

    .. code:: yaml

      name: Test
      # [...] recipe mandatory keys
      waters:
        Burton on Trent, UK:
          amount: 20.0
          calcium: 295.0
          magnesium: 45.0
          sodium: 55.0
          sulfate: 725.0
          chloride: 25.0
          bicarbonate: 300.0
          ph: 8.0


Examples
--------

  .. raw:: html

    <div style="display: flex">
    <div style="display: inline-block;max-width: 49%;margin:1px">

  .. code:: xml

    <RECIPE>
      <NAME>Dry Stout</NAME>
      <VERSION>1</VERSION>
      <TYPE>All Grain</TYPE>
      <BREWER>Brad Smith</BREWER>
      <BATCH_SIZE>18.93</BATCH_SIZE>
      <BOIL_SIZE>20.82</BOIL_SIZE>
      <BOIL_TIME>60.0</BOIL_TIME>
      <EFFICIENCY>72.0</EFFICIENCY>
      <TASTE_NOTES>
        Nice dry Irish stout with a warm body but low starting gravity much like
        the famous drafts.
      </TASTE_NOTES>
      <RATING>41</RATING>
      <DATE>3 Jan 04</DATE>
      <OG>1.036</OG>
      <FG>1.012</FG>
      <CARBONATION>2.1</CARBONATION>
      <CARBONATION_USED>Kegged</CARBONATION_USED>
      <AGE>24.0</AGE>
      <AGE_TEMP>17.0</AGE_TEMP>
      <FERMENTATION_STAGES>2</FERMENTATION_STAGES>
      <STYLE>
        <NAME>Dry Stout</NAME>
        <CATEGORY>Stout</CATEGORY>
        <CATEGORY_NUMBER>16</CATEGORY_NUMBER>
        <STYLE_LETTER>A</STYLE_LETTER>
        <STYLE_GUIDE>BJCP</STYLE_GUIDE>
        <VERSION>1</VERSION>
        <TYPE>Ale</TYPE>
        <OG_MIN>1.035</OG_MIN>
        <OG_MAX>1.050</OG_MAX>
        <FG_MIN>1.007</FG_MIN>
        <FG_MAX>1.011</FG_MAX>
        <IBU_MIN>30.0</IBU_MIN>
        <IBU_MAX>50.0</IBU_MAX>
        <COLOR_MIN>35.0</COLOR_MIN>
        <COLOR_MAX>200.0</COLOR_MAX>
        <ABV_MIN>3.2</ABV_MIN>
        <ABV_MAX>5.5</ABV_MAX>
        <CARB_MIN>1.6</CARB_MIN>
        <CARB_MAX>2.1</CARB_MAX>
        <NOTES>
          Famous Irish Stout. Dry, roasted, almost coffee like flavor. Often
          soured with pasteurized sour beer. Full body perception due to flaked
          barley, though starting gravity may be low. Dry roasted flavor.
        </NOTES>
      </STYLE>
      <HOPS>
        <HOP>
          <NAME>Goldings, East Kent</NAME>
          <VERSION>1</VERSION>
          <ALPHA>5.0</ALPHA>
          <AMOUNT>0.0638</AMOUNT>
          <USE>Boil</USE>
          <TIME>60.0</TIME>
          <NOTES>Great all purpose UK hop for ales, stouts, porters</NOTES>
        </HOP>
      </HOPS>
      <FERMENTABLES>
        <FERMENTABLE>
          <NAME>Pale Malt (2 row) UK</NAME>
          <VERSION>1</VERSION>
          <AMOUNT>2.27</AMOUNT>
          <TYPE>Grain</TYPE>
          <YIELD>78.0</YIELD>
          <COLOR>3.0</COLOR>
          <ORIGIN>United Kingdom</ORIGIN>
          <SUPPLIER>Fussybrewer Malting</SUPPLIER>
          <NOTES>All purpose base malt for English styles</NOTES>
          <COARSE_FINE_DIFF>1.5</COARSE_FINE_DIFF>
          <MOISTURE>4.0</MOISTURE>
          <DIASTATIC_POWER>45.0</DISASTATIC_POWER>
          <PROTEIN>10.2</PROTEIN>
          <MAX_IN_BATCH>100.0</MAX_IN_BATCH>
        </FERMENTABLE>
        <FERMENTABLE>
          <NAME>Barley, Flaked</NAME>
          <VERSION>1</VERSION>
          <AMOUNT>0.91</AMOUNT>
          <TYPE>Grain</TYPE>
          <YIELD>70.0</YIELD>
          <COLOR>2.0</COLOR>
          <ORIGIN>United Kingdom</ORIGIN>
          <SUPPLIER>Fussybrewer Malting</SUPPLIER>
          <NOTES>Adds body to porters and stouts, must be mashed</NOTES>
          <COARSE_FINE_DIFF>1.5</COARSE_FINE_DIFF>
          <MOISTURE>9.0</MOISTURE>
          <DIASTATIC_POWER>0.0</DISASTATIC_POWER>
          <PROTEIN>13.2</PROTEIN>
          <MAX_IN_BATCH>20.0</MAX_IN_BATCH>
          <RECOMMEND_MASH>TRUE</RECOMMEND_MASH>
        </FERMENTABLE>
        <FERMENTABLE>
          <NAME>Black Barley</NAME>
          <VERSION>1</VERSION>
          <AMOUNT>0.45</AMOUNT>
          <TYPE>Grain</TYPE>
          <YIELD>78.0</YIELD>
          <COLOR>500.0</COLOR>
          <ORIGIN>United Kingdom</ORIGIN>
          <SUPPLIER>Fussybrewer Malting</SUPPLIER>
          <NOTES>Unmalted roasted barley for stouts, porters</NOTES>
          <COARSE_FINE_DIFF>1.5</COARSE_FINE_DIFF>
          <MOISTURE>5.0</MOISTURE>
          <DIASTATIC_POWER>0.0</DISASTATIC_POWER>
          <PROTEIN>13.2</PROTEIN>
          <MAX_IN_BATCH>10.0</MAX_IN_BATCH>
        </FERMENTABLE>
      </FERMENTABLES>
      <MISCS>
        <MISC>
          <NAME>Irish Moss</NAME>
          <VERSION>1</VERSION>
          <TYPE>Fining</TYPE>
          <USE>Boil</USE>
          <TIME>15.0</TIME>
          <AMOUNT>0.010</AMOUNT>
          <NOTES>
            Used as a clarifying agent during the last few minutes of the boil
          </NOTES>
        </MISC>
      </MISCS>
      <WATERS>
        <WATER>
          <NAME>Burton on Trent, UK</NAME>
          <VERSION>1</VERSION>
          <AMOUNT>20.0</AMOUNT>
          <CALCIUM>295.0</CALCIUM>
          <MAGNESIUM>45.0</MAGNESIUM>
          <SODIUM>55.0</SODIUM>
          <SULFATE>725.0</SULFATE>
          <CHLORIDE>25.0</CHLORIDE>
          <BICARBONATE>300.0</BICARBONATE>
          <PH>8.0</PH>
          <NOTES>
            Use for distinctive pale ales strongly hopped.
            Very hard water accentuates the hops flavor. Example: Bass Ale
          </NOTES>
        </WATER>
      </WATERS>
      <YEASTS>
        <YEAST>
          <NAME>Irish Ale</NAME>
          <TYPE>Ale</TYPE>
          <VERSION>1</VERSION>
          <FORM>Liquid</FORM>
          <AMOUNT>0.250</AMOUNT>
          <LABORATORY>Wyeast Labs</LABORATORY>
          <PRODUCT_ID>1084</PRODUCT_ID>
          <MIN_TEMPERATURE>16.7</MIN_TEMPERATURE>
          <MAX_TEMPERATURE>22.2</MAX_TEMPERATURE>
          <ATTENUATION>73.0</ATTENUATION>
          <NOTES>
            Dry, fruity flavor characteristic of stouts.  Full bodied, dry,
            clean flavor.
          </NOTES>
          <BEST_FOR>Irish Dry Stouts</BEST_FOR>
          <FLOCCULATION>Medium</FLOCCULATION>
        </YEAST>
      </YEASTS>
      <MASH>
        <NAME>Single Step Infusion, 68 C</NAME>
        <VERSION>1</VERSION>
        <GRAIN_TEMP>22.0</GRAIN_TEMP>
        <MASH_STEPS>
          <MASH_STEP>
            <NAME>Conversion Step, 68C </NAME>
            <VERSION>1</VERSION>
            <TYPE>Infusion</TYPE>
            <STEP_TEMP>68.0</STEP_TEMP>
            <STEP_TIME>60.0</STEP_TIME>
            <INFUSE_AMOUNT>10.0</INFUSE_AMOUNT>
          </MASH_STEP>
        </MASH_STEPS>
      </MASH>
    </RECIPE>

  .. raw:: html

    </div>
    <div style="display: inline-block;max-width: 49%;margin: 1px">

  .. code:: yaml

        # equivalent in YAML
        name: Dry Stout
        type: All Grain
        brewer: Brad Smith
        batch_size: 18.93
        boil_size: 20.82
        boil_time: 60
        efficiency: 72.0
        taste_notes: >
          Nice dry Irish stout with a warm body but low starting gravity much
          like the famous drafts
        rating: 41
        date: 3 Jan 04
        og: 1.036
        fg: 1.012
        carbonation: 2.1
        carbonation_used: Kegged
        age: 24
        age_temp: 17.0
        fermentation_stages: 2
        style:
          name: Dry Stout
          category: Stout
          category_number: 16
          style_letter: A
          style_guide: BJCP
          type: Ale
          og_min: 1.035
          og_max: 1.050
          fg_min: 1.007
          fg_max: 1.011
          ibu_min: 30.0
          ibu_max: 50.0
          color_min: 35.0
          color_max: 200.0
          abv_min: 3.2
          abv_max: 5.5
          carb_min: 1.6
          carb_max: 2.1
          notes: >
            Famous Irish Stout. Dry, roasted, almost coffee like flavor. Often
            soured with pasteurized sour beer. Full body perception due to flaked
            barley, though starting gravity may be low. Dry roasted flavor.
        hops:
          Goldings, East Kent:
            alpha: 5.0
            use: boil
            time: 60.0
            amount: 0.0638
            notes: Great all purpose UK hop for ales, stouts, porters
        fermentables:
          Pale Malt (2 row) UK:
            amount: 2.27
            type: Grain
            yield: 78.0
            color: 3.0
            origin: United Kingdom
            supplier: Fussybrewer Malting
            notes: All purpose base malt for English styles
            coarse_fine_diff: 1.5
            moisture: 4.0
            diastatic_power: 45.0
            protein: 10.2
            max_in_batch: 100.0
          Barley, Flaked:
            amount: 0.91
            type: grain
            yield: 70.0
            color: 2.0
            origin: United Kingdom
            supplier: Fussybrewer Malting
            notes: Adds body to porters and stouts, must be mashed
            coarse_fine_diff: 1.5
            moisture: 9.0
            diastatic_power: 0.0
            protein: 13.2
            max_in_batch: 20.0
            recommend_mash: true
          Black Barley:
            amount: 0.45
            type: grain
            yield: 78.0
            color: 500
            origin: United Kingdom
            supplier: Fussybrewer Malting
            notes: Unmalted roasted barley for stouts, porters
            coarse_fine_diff: 1.5
            moisture: 5.0
            diastatic_power: 0.0
            protein: 13.2
            max_in_batch: 10.0
        miscs:
          Irish Moss:
            type: Fining
            use: Boil
            time: 15
            amount: 0.010
            notes: >
              Used as a clarifying agent during the last few minutes of the boil
        waters:
          Burton on Trent, UK:
            amount: 20.0
            calcium: 295.0
            magnesium: 45.0
            sodium: 55.0
            sulfate: 725.0
            chloride: 25.0
            bicarbonate: 300.0
            ph: 8.0
            notes: >
              Use for distinctive pale ales strongly hopped.
              Very hard water accentuates the hops flavor. Example: Bass Ale
        yeasts:
          Irish Ale:
            type: ale
            form: Liquid
            amount: 0.25
            laboratory: wyeast labs
            product_id: 1084
            min_temperature: 16.7
            max_temperature: 22.2
            attenuation: 73.0
            notes: >
              Dry, fruity flavor characteristic of stouts.
              Full bodied, dry, clean flavor.
            best_for: irish dry stouts
            flocculation: medium
      mash:
        name: Single Step infusion, 68 C
        grain_temp: 22.0
        mash_steps:
          Conversion step, 68C:
            type: infusion
            step_temp: 68.0
            step_time: 60.0
            infuse_amount: 10.0

  .. raw:: html

    </div>
    </div>

---------

**Simple recipe**

  .. code:: yaml

        name: Dry Stout
        type: All Grain
        brewer: Brad Smith
        batch_size: 18.93
        boil_size: 20.82
        boil_time: 60
        style: Dry Stout
        hops:
          Goldings, East Kent:
            alpha: 5.0
            use: boil
            time: 60.0
            amount: 0.0638
        fermentables:
          Pale Malt (2 row) UK:
            amount: 2.27
            type: Grain
            yield: 78.0
            color: 3.0
          Barley, Flaked:
            amount: 0.91
            type: grain
            yield: 70.0
            color: 2.0
          Black Barley:
            amount: 0.45
            type: grain
            yield: 78.0
            color: 500
        miscs:
          Irish Moss:
            type: Fining
            use: Boil
            time: 15
            amount: 0.010
        yeasts:
          Irish Ale:
            type: ale
            form: Liquid
            amount: 0.25
      mash:
        name: Single Step infusion, 68 C
        grain_temp: 22.0
        mash_steps:
          Conversion step, 68C:
            type: infusion
            step_temp: 68.0
            step_time: 60.0

Implementations
---------------

- Python http://pybeeryaml.readthedocs.io/
