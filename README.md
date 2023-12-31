# HPG IncidentAI Dataset
Official code and data for our paper "**Towards Safer Operations: An Expert-involved Dataset of High-Pressure Gas Incidents for Preventing Future Failures**" - EMNLP 2023 (Industry Track)

## Overview

This project provides a new Japanese IncidentAI dataset for safety prevention on high-pressure gas plant domain. Our dataset comprises NLP three tasks: **Named Entity Recognition (NER)**, **Cause-Effect Extraction (CE)**, and **Information Retrieval (IR)**.
The original dataset was collected from publicly available [reports of high-gas incidents published in 2022 by the High-Pressure Gas Safety Institute of Japan](https://www.khk.or.jp/public_information/incident_investigation/hpg_incident/incident_db.html).

The dataset is annotated by domain experts who have at least six years of practical experience as high-pressure gas conservation managers. These experts possess qualifications as high-pressure gas production safety managers, a national certification demonstrating a certain level of knowledge and experience necessary to ensure the safety of high-pressure gas manufacturing facilities.

The detailed descriptions of each annotation definition for NER, CE and IR can be accessed in our annotation guideline, `HPG_Annotaion_Guideline.pdf`.


## Named Entity Recognition

### Definitions
The NER dataset include six type of entities as following.
| Entity    | Descriptions                                                                                                                                                                                                                                                                                                                   | Examples                                                                                                                                                                                                                          |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Products  | Various gases.<br>Gaseous state at normal temperature and pressure.<br>Nouns.<br><br>※Do not tag items that are not general (things that do not appear even if you search the Web).<br>                                                                                                                                       | Mixed gas<br>Flammable gas<br>Refrigerant gas<br>Inert gas<br>Liquefied petroleum gas<br>Carbon dioxide gas<br>Sulphur dioxide gas<br>Liquefied petroleum gas<br>Freon<br>Hydrogen, Carbon monoxide, Acetylene, Methane, Ethylene |
| Chemicals | Chemical substances, reactants, and materials (other than gases) used in gas generation and process management。<br>Items not included in the above Products.<br>Nouns.                                                                                                                                                        | Water, water droplets, rainwater, wash water, hot water, pure water<br>H2O<br>Benzene<br>Austenitic stainless steel<br>Lubricating oil<br>C4-C6<br>Hydrocarbons                                                                   |
| Storages  | General equipment where above Products and Chemicals come into contact.<br><br>※Include equipment such as supports and insulators.<br>※Include expressions that indicate the entire plant or facility.<br>※Do not include expressions indicating parts such as entrances and exits if they are placed at the end of a word. | Tank<br>Maturation furnace<br>Refining tower<br>Dehumidification tower<br>Separation tower<br>Heat exchanger<br>Piping<br>Valve<br>Gasket<br>Flange<br>BTX manufacturing equipment<br>Butadiene plant                             |
| Incidents | Incidents that resulted in or caused an accident, regardless of severity. Include only incidents that actually occurred, and do not include situations that did not lead to an incident.                                                                                                                                       | Explosion<br>Seepage<br>Leakage<br>Fire<br>Serious injury<br>Death<br>Degradation<br>Concentration<br>Issuing of an alert, detection, awareness, (alarm) activation                                                               |
| Process   | Handling of gas, and unit operations related to gas.<br>Abnormal processes are included in Incidents.                                                                                                                                                                                                                          | Filling<br>Distillation<br>Extraction<br>Reaction<br>Recovery<br>Mixing<br>Sealing<br>Nitrogen purge                                                                                                                              |
| Tests     | Inspection devices and inspection actions outside the production process line.<br>Do not include inspection items such as XX concentration.                                                                                                                                                                                    | Inspection, visual inspection, three-month inspection<br>Detailed inspection, leakage inspection<br>Freon checker<br>Leak test<br>Analysis<br>Patrol                                                                              |

### Data Format
@Nathan: Please provide the explanation about the data format in the file uploaded in directory: `./NER/`

### Example Data
#### Japanese (Original)
![ner_exmaple_jp](assets/ner_sample_jp.png)

#### English (Translated)
![ner_exmaple_en](assets/ner_sample_en.png)



## Cause-Effect Extraction
### Definitions
The CE dataset define the span following five type of entities.

| Entity          | Descriptions                                                                                                                                                                                                                                                                                                                   | Examples                                                                                                 |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| Event_Leak      | Various gases.<br>Gaseous state at normal temperature and pressure.<br>Nouns.<br><br>※Do not tag items that are not general (things that do not appear even if you search the Web).<br>                                                                                                                                       | Hydrogen and aniline leakage                                                                             |
| Event_others    | Chemical substances, reactants, and materials (other than gases) used in gas generation and process management。<br>Items not included in the above Products.<br>Nouns.                                                                                                                                                        | It is estimated that hydrogen, which<br>has a low ignition energy, was ignited<br>by static electricity. |
| Damage_Property | General equipment where above Products and Chemicals come into contact.<br><br>※Include equipment such as supports and insulators.<br>※Include expressions that indicate the entire plant or facility.<br>※Do not include expressions indicating parts such as entrances and exits if they are placed at the end of a word. | Container ruptures.                                                                                      |
| Damage_Human    | Incidents that resulted in or caused an accident, regardless of severity. Include only incidents that actually occurred, and do not include situations that did not lead to an incident.                                                                                                                                       | One employee injured left thigh and<br>left ear.                                                         |
| Cause           | Tag sentences that confirm the event<br>causing Event_Leak and<br>Event_others. Target not only direct<br>causes but also indirect causes (e.g.,<br>Cause's Cause)。<br>In case of ignition or explosion, the<br>three elements of combustion<br>(combustibles, oxygen, and heat)<br>shall be noted cause.                     | As a result of reduced tightening<br>torque in some of the flange sections<br>cooled by hydrogen         |

### Data Format
[//]: # (@Hubert: Please provide the explanation about the data format in the file uploaded in directory: `./CE/`)

To make the Cause-Effect Extraction data more accessible, we prepare train/test splits in JSON format. Each item contains three fields:

- "text": original text of the data item
- "tags": list of tags for annotated spans
- "spans": list of annotated spans

From these fields, you can convert train/test data to standard NER (sequence labeling) or extractive QA format (SQuAD format).
```
{
	"text": "2006-217 ポリブデン製造設備の水添反応器において、触媒再生作業中、内部を冷却するため水素ガスを送っていたところ爆発音がしたため、作業員が現場に急行したところ反応器の下部配管フランジ部より発火していた。 このため消火器で火を消し、その後公設消防が放水して当該部位付近を冷却した。この火災により、リアクター下部配管の保温材が焼損した。 原因は、本作業中に当該下部配管を取り替えたが、接続する際に本来は直径111ｍｍのパッキンを取付けるところ、誤って95mmのパッキンを取付けてしまったことである。 このため、水素が漏えいし静電気により着火し火災となったとみられる。今後は、作業マニュアルを見直し、作業員の教育を徹底することとした。",
	"tags": [
		"Event_others",
		"Damage_Property",
		"Cause",
		"Event_Leak",
		"Cause",
		"Event_others"
	],
	"spans": [
		"ポリブデン製造設備の水添反応器において、触媒再生作業中、内部を冷却するため水素ガスを送っていたところ爆発音がしたため、作業員が現場に急行したところ反応器の下部配管フランジ部より発火していた",
		"この火災により、リアクター下部配管の保温材が焼損した",
		"本作業中に当該下部配管を取り替えたが、接続する際に本来は直径111ｍｍのパッキンを取付けるところ、誤って95mmのパッキンを取付けてしまったことである",
		"水素が漏えいし",
		"静電気により",
		"着火し火災となったとみられる"
	]
}
```

### Example Data
#### Japanese (Original)
![ce_exmaple_jp](assets/ce_sample_jp.png)

#### English (Translated)
![ce_exmaple_en](assets/ce_sample_en.png)


## Information Retrieval
### Definitions
The IR dataset defines `Attributes` and their `Labels` for a given accident descriptions as following table.

| Attribute                                                                | Label                                                                                                                                                                                                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Types of<br>high-pressure gas                                            | a. Flammable (or flame<br>retardant) gas<br>b. Toxic gas<br>c. Satisfies a and b<br>d. Not applicable                                                                                                | The high-pressure gas that caused the<br>reported accident was classified from the<br>perspective of danger in the event of an<br>accident. Cases where the gas could not be<br>identified were included under “d. Not<br>applicable”.<br>The definition of flammable gas and toxic gas<br>shall conform to the High Pressure Gas Safety<br>Act in Japan.                                                                                                                                                       |
| Cause of accident                                                        | a. Equipment Factor<br>b. Human Factor<br>c. External factor<br>d. Other factor                                                                                                                      | The events that caused or triggered the accident were classified. Equipment factors<br>refer to those caused by initial defects in parts<br>built into the equipment. Human factors refer<br>to errors made in operation or judgment by<br>people on site. External factors indicate those<br>caused by events from outside the equipment,<br>such as falling objects.                                                                                                                                            |
| Accident Results                                                         | a. Leakage<br>b. Fires and explosions<br>c. a. and property damage<br>d. a. and human casualties<br>e. b and property damage<br>f. b and human casualties<br>g. Property damage and human casualties | The events that occurred as a result of the<br>accident were classified. Physical and human<br>damage were only considered if they occurred<br>as secondary events, such as gas leaks or fires.<br>Property damage ： Accidents resulting in<br>damage to equipment or facilities due to fire<br>or explosion<br>※Do not include damage to equipment or<br>other items that caused the accident.<br>Human casualties ： Accidents resulting in<br>health hazards to humans due to leakage, fire,<br>or explosion |
| Time span from<br>cause to effect                                        | a. Sudden<br>b. Long-term<br>c. Unknown                                                                                                                                                              | The classification was made based on the time<br>from when the cause or trigger of the accident<br>occurred until the accident event took place.<br>Sudden ： Accidents where the results are<br>caused generally within a few minutes to<br>several tens of minutes from the occurrence<br>of the cause.                                                                                                                                                                                                         |
| Operational status<br>of equipment at the<br>time of cause<br>occurrence | a, During steady-state operation<br>b. During non-steady state operation<br>c. During maintenance<br>d. Other situations.                                                                            | The classification was made based on the<br>operational status of the equipment at the<br>time of the accident.<br>Non-steady state operation refers to<br>operating conditions that differ from normal<br>operation, such as immediately after the<br>equipment starts running or during test<br>operation.                                                                                                                                                                                                      |


### Example Data
![ir_exmaple](assets/ir_sample.png)

## Citation
If you find our work helpful, please cite us:
```
@misc{inoue2023safer,
      title={Towards Safer Operations: An Expert-involved Dataset of High-Pressure Gas Incidents for Preventing Future Failures}, 
      author={Shumpei Inoue and Minh-Tien Nguyen and Hiroki Mizokuchi and Tuan-Anh D. Nguyen and Huu-Hiep Nguyen and Dung Tien Le},
      year={2023},
      eprint={2310.12074},
      archivePrefix={arXiv},
      primaryClass={cs.CL}
}
```
