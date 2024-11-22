In the most simple flow, a provider creates a request and instructs a fulfiller that they should perform it. In this scenario, the placer is confident based on pre-coordinated buisiness and IT agreements that the performer won't decline the request.

Based on the workflow of interest, the performer may or may not notify the requestor of an 'outcome' at a later time. 

{% include img.html img="request-simplest.png" %}

'''plantuml
autonumber
skinparam sequenceMessageAlign center

Bob -> Alice : hello

Alice -> Bob 

Alice <- Alice : //Test message//
'''
