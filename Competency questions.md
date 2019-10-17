# Competencey questions for the IoMusT ontology

The given SPARQL are _examples_ that may be reinterpreted and reused for applications.

1. Which type of Musical Things are used by the local and remote performers during the live concert?
```
SELECT * WHERE {
  ?device rdf:type ?type.
  FILTER (?type != iomust:MusicalThing)
}
```
2. How many Musical Things used by the audience provide haptic feedback?
```
SELECT ?mt (count(distinct ?mt) as ?count)
WHERE {
    ?mt rdf:type iomust:MusicalThing;
        iot:isInvolvedIn _:AudienceApplication;
        sosa:hosts ?actuator.
    ?actuator rdf:type sosa:Actuator;
        sosa:actsOnProperty _:haptic_Feedback.
}
```
3. What smart instruments are controlling the smartphones used by the audience?
```
SELECT ?si
WHERE {
    ?si rdf:type iomust:SmartInstrument;
        iot:isInvolvedIn _:AudienceApplication.
    _:AudienceApplication iot:produces _:event.
    _:event event:agent ?si;
        event:factor ?smartphone.
    ?smartphone rdf:type iot:SmartThing.
}
```
4. What is the mood of the music at a given time during the live performance?

_Additional concepts describing the mood are needed._

5. How many audience members are actively participating in the music creation process thanks to their Musical Things?
```
SELECT (count(distinct ?member) as ?count)
WHERE {
    ?member a foaf:Person;
       iot:isInvolvedIn/iot:produces _:event.
    _:event event:agent ?member.
}
```

6. Which kind of stage equipment is used at a given time during the concert?
```
SELECT DISTINCT ?se_type
WHERE {
    ?se rdf:type iomust:StageEquipment, co:collection.
    ?item co:elementOf ?se;
       rdf:type ?se_type;
       iot:isInvolvedIn _:concert.
    _:concert iot:produces/event:time/timeline:starts <GIVEN_TIME>.
}
```

7. Which gestural and biometric parameters are tracked from the audience during the live performance?
```
SELECT DISTINCT ?param
WHERE {
  ?wt rdf:type iomust:WearableMusicalThing, sosa:Platform;
     iot:isInvolvedIn _:livePerformance;
     sosa:hosts ?sensor.
  ?sensor rdf:type sosa:Sensor;
     sosa:observes ?param.
}
```

8. How many and which kind of networks are used during a performance?
```
SELECT ?netw (count(?netw) as ?count)
WHERE {
    ?thing rdf:type iot:ConnectedThing;
       iot:isInvolvedIn _:performance;
       iot:hasConnection ?netw.
    ?netw rdf:type iot:Connection.
}
GROUP BY ?netw
```

9. What pedagogical applications are available for a smart violin?
```
SELECT DISTINCT ?app WHERE {
    ?app rdf:type iomust:MusicalThingApplication, iot:LearningApplication.
    ns:SmartViolinResource iot:isInvolvedIn ?app.
}
```

10. With which music content repository a smart ukulele can interact?

_Additional concepts describing the content repositories are needed._

11. Which services are available for a smart guitar and what are their purposes?
```
SELECT * WHERE {
    ?app rdf:type ?type.
    ns:SmartGuitarResource iot:isInvolvedIn ?app.
}
```

12. What type of sensors and actuators compose a smart musical instrument or a musical haptic wearable?
```
SELECT * WHERE {
  ?wt rdf:type iomust:SmartMusicalThing, sosa:Platform;
     sosa:hosts ?sa.
  ?sa rdf:type ?sa_type.
  OPTIONAL {
    ?sa sosa:observes ?param. }.
  OPTIONAL {
    ?sa sosa:actsOnProperty ?param .}.
}
```
