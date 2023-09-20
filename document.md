
RASA FORM

Tham khảo: https://www.youtube.com/watch?v=aBWVYfKzdJQ

1. nlu.yml
```
version: "3.1"
nlu:
- intent: greet
  examples: |
    - hey
    - hello
    - hi
    - hello there
    - good morning
    - good evening
    - moin
    - hey there
    - let's go
    - hey dude
    - goodmorning
    - goodevening
    - good afternoon
    - ask_phone
    - repeat_phone
- intent: goodbye
  examples: |
    - cu
    - good by
    - cee you later
    - good night
    - bye
    - goodbye
    - have a nice day
    - see you around
    - bye bye
    - see you later
- intent: affirm
  examples: |
    - yes
    - y
    - indeed
    - of course
    - that sounds good
    - correct
- intent: deny
  examples: |
    - no
    - n
    - never
    - I don't think so
    - don't like that
    - no way
    - not really
- intent: mood_great
  examples: |
    - perfect
    - great
    - amazing
    - feeling like a king
    - wonderful
    - I am feeling very good
    - I am great
    - I am amazing
    - I am going to save the world
    - super stoked
    - extremely good
    - so so perfect
    - so good
    - so perfect
- intent: mood_unhappy
  examples: |
    - my day was horrible
    - I am sad
    - I don't feel very well
    - I am disappointed
    - super sad
    - I'm so sad
    - sad
    - very sad
    - unhappy
    - not good
    - not very good
    - extremly sad
    - so saad
    - so sad
- intent: bot_challenge
  examples: |
    - are you a bot?
    - are you a human?
    - am I talking to a bot?
    - am I talking to a human?
- intent: phone
  examples: |
    - [7559854598](number)
    - This is my phone number [2365894561](number)
    - Here you go [9874562159](number)
    - my phone is [1234567870](number)
    - phone number is [0918152524](number)
- intent: repeat_phone
  examples: |
    - what is my phone number
    - tell me my phone number
    - repeat my phone number
    - can you repeat my phone number
    - my phone number is?
```
2. rules.yml 
```
version: "3.1"

rules:

- rule: Say goodbye anytime the user says goodbye
  steps:
  - intent: goodbye
  - action: utter_goodbye

- rule: Say 'I am a bot' anytime the user challenges
  steps:
  - intent: bot_challenge
  - action: utter_iamabot
```

3. stories.yml
```
version: "3.1"

stories:
- story: interactive 1 
  steps: 
  - intent: greet 
  - action: utter_ask_phone
  - intent: phone
    entities: 
    - number: 7896541230
  - slot_was_set: 
    - phone: 9874561230
  - action: action_say_phone 
```
4. domain.yml 
```
version: '3.1'

intents:
- affirm
- bot_challenge
- deny
- goodbye
- greet
- mood_great
- mood_unhappy
- phone
- repeat_phone

entities:
- number

forms:
  number_form:
    required_slots:
    - phone

slots:
  phone:
    type: text
    influence_conversation: true
    mappings:
    - type: from_entity
      entity: number

responses:
  utter_greet:
  - text: Hey! How are you?
  utter_ask_phone:
  - text: Can I get your phone number please?
  utter_cheer_up:
  - text: 'Here is something to cheer you up:'
    image: https://i.imgur.com/nGF1K8f.jpg
  utter_did_that_help:
  - text: Did that help you?
  utter_happy:
  - text: Great, carry on!
  utter_goodbye:
  - text: Bye
  utter_iamabot:
  - text: I am a bot, powered by Rasa.

actions:
- action_say_phone

session_config:
  session_expiration_time: 60
  carry_over_slots_to_new_session: true

```