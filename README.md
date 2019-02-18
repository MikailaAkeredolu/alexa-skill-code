
#Please visit https://alexa.design/cookbook for additional examples on implementing slots

const Alexa = require('ask-sdk-core');

const LaunchRequestHandler = {
    
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'LaunchRequest';
    },
    handle(handlerInput) {
        const speechText = `Welcome to Pizza King, To get started you can say something like i would like to order a pizza`;
        return handlerInput.responseBuilder
            .speak(speechText)
            .reprompt(speechText)
            .getResponse();
    }
};


const OrderHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'IntentRequest'
            && handlerInput.requestEnvelope.request.intent.name === 'OrderIntent';
    },
    handle(handlerInput) {
        
        const slots = handlerInput.requestEnvelope.request.intent.slots;
      const {size, crust, topping} = slots;
      
 if (!size.value || !(size.value === 'extra large' || size.value === 'large' || size.value === 'medium' || size.value === 'small')) {
    const slotToElicit = 'size';
    const speechOutput = ``;
    return handlerInput.responseBuilder
        .speak(speechOutput)
        .reprompt(speechOutput)
        .addElicitSlotDirective(slotToElicit) 
        .getResponse();
}

 if (!crust.value || !(crust.value === 'hand tossed' || crust.value === 'thin crust' || crust.value === 'sicilian' || crust.value === 'deep dish')) {
    const slotToElicit = 'size';
    const speechOutput = ``;
    return handlerInput.responseBuilder
        .speak(speechOutput)
         .reprompt(speechOutput)
        .addElicitSlotDirective(slotToElicit)
        .getResponse();
}

 if (!topping.value || !(topping.value === 'ham' || topping.value === 'sausage' || topping.value === 'cheese' || topping.value === 'pepperoni')) {
    const slotToElicit = 'size';
    const speechOutput = ``;
    return handlerInput.responseBuilder
        .speak(speechOutput)
        .reprompt(speechOutput)
        .addElicitSlotDirective(slotToElicit)
        .getResponse();
}

    const speechOutput = `<audio src='soundbank://soundlibrary/home/amzn_sfx_doorbell_chime_02'/> You mentioned,
    that you wanted a ${size.value} , ${topping.value } pizza with ${crust.value}.
    Your total will be forty dollars and will be ready in fifteen minutes <audio src='soundbank://soundlibrary/foley/amzn_sfx_swoosh_cartoon_fast_01'/>`;
    return handlerInput.responseBuilder
        .speak(speechOutput)
        .reprompt(speechOutput)
        .getResponse();

     }
};

const HelpIntentHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'IntentRequest'
            && handlerInput.requestEnvelope.request.intent.name === 'AMAZON.HelpIntent';
    },
    handle(handlerInput) {
        const speechText = 'You can say something like, I would like to order a pizza! How can I help?';

        return handlerInput.responseBuilder
            .speak(speechText)
            .reprompt(speechText)
            .getResponse();
    }
};

const CancelAndStopIntentHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'IntentRequest'
            && (handlerInput.requestEnvelope.request.intent.name === 'AMAZON.CancelIntent'
                || handlerInput.requestEnvelope.request.intent.name === 'AMAZON.StopIntent');
    },
    handle(handlerInput) {
        const speechText = 'Goodbye!';
        return handlerInput.responseBuilder
            .speak(speechText)
            .getResponse();
    }
};

const SessionEndedRequestHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'SessionEndedRequest';
    },
    handle(handlerInput) {
        return handlerInput.responseBuilder.getResponse();
    }
};


const ErrorHandler = {
    canHandle() {
        return true;
    },
    handle(handlerInput, error) {
        console.log(`~~~~ Error handled: ${error.message}`);
        const speechText = `Sorry, I couldn't understand what you said. Please try again.`;

        return handlerInput.responseBuilder
            .speak(speechText)
            .reprompt(speechText)
            .getResponse();
    }
};

#This handler acts as the entry point for your skill, routing all request and response
# payloads to the handlers above. 
exports.handler = Alexa.SkillBuilders.custom()
    .addRequestHandlers(
        LaunchRequestHandler,
        OrderHandler,
        HelpIntentHandler,
        CancelAndStopIntentHandler,
        SessionEndedRequestHandler
        //IntentReflectorHandler
        ) // make sure IntentReflectorHandler is last so it doesn't override your custom intent handlers
    .addErrorHandlers(
        ErrorHandler)
    .lambda();




#INTERACTION MODEL BELOW

{
    "interactionModel": {
        "languageModel": {
            "invocationName": "pizza king",
            "intents": [
                {
                    "name": "AMAZON.CancelIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.HelpIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.StopIntent",
                    "samples": []
                },
                {
                    "name": "OrderIntent",
                    "slots": [
                        {
                            "name": "size",
                            "type": "SIZE_OF_PIZZA",
                            "samples": [
                                "I will take a {size} pizza",
                                "I will take a {size}",
                                "{size}"
                            ]
                        },
                        {
                            "name": "crust",
                            "type": "TYPE_OF_CRUST",
                            "samples": [
                                "{crust} will be fine",
                                "{crust} please",
                                "let me get a {crust}",
                                "I will take a {crust}",
                                "{crust}"
                            ]
                        },
                        {
                            "name": "topping",
                            "type": "TYPE_OF_TOPPING",
                            "samples": [
                                "{topping} please",
                                "I will take a {topping}",
                                "{topping}"
                            ]
                        }
                    ],
                    "samples": [
                        "i would like to order pizzas",
                        "klet me get a pizza",
                        "I want to place an order for pizza",
                        "i wann place an order",
                        "order a pizza for me yo",
                        "yo i wanna order a pizza",
                        "let me get a pizza",
                        "I want to order pizza",
                        "I would like to order a pizza",
                        "order me a pizza",
                        "I would like to place an order",
                        "i want some pizza",
                        "i want to order some pizza"
                    ]
                },
                {
                    "name": "AMAZON.NavigateHomeIntent",
                    "samples": []
                }
            ],
            "types": [
                {
                    "name": "SIZE_OF_PIZZA",
                    "values": [
                        {
                            "name": {
                                "value": "extra large"
                            }
                        },
                        {
                            "name": {
                                "value": "large"
                            }
                        },
                        {
                        "name": {
                                "value": "medium"
                            }
                        },
                        {
                            "name": {
                                "value": "small"
                            }
                        }
                    ]
                },
                {
                    "name": "TYPE_OF_CRUST",
                    "values": [
                        {
                            "name": {
                                "value": "deep dish"
                            }
                        },
                        {
                            "name": {
                                "value": "sicilian"
                            }
                        },
                        {
                            "name": {
                                "value": "thin crust"
                            }
                        },
                        {
                            "name": {
                                "value": "hand tossed"
                            }
                        }
                    ]
                },
                {
                    "name": "TYPE_OF_TOPPING",
                    "values": [
                        {
                            "name": {
                                "value": "cheese"
                            }
                        },
                        {
                            "name": {
                                "value": "sausage"
                            }
                        },
                        {
                            "name": {
                                "value": "pepperoni"
                            }
                        },
                        {
                            "name": {
                                "value": "ham"
                            }
                        }
                    ]
                }
            ]
        },
        "dialog": {
            "intents": [
                {
                    "name": "OrderIntent",
                    "confirmationRequired": false,
                    "prompts": {},
                    "slots": [
                        {
                            "name": "size",
                            "type": "SIZE_OF_PIZZA",
                            "confirmationRequired": false,
                            "elicitationRequired": true,
                            "prompts": {
                                "elicitation": "Elicit.Slot.1218686242843.959247103465"
                            }
                        },
                        {
                            "name": "crust",
                            "type": "TYPE_OF_CRUST",
                            "confirmationRequired": false,
                            "elicitationRequired": true,
                            "prompts": {
                                "elicitation": "Elicit.Slot.651250309317.169830581825"
                            }
                        },
                        {
                            "name": "topping",
                            "type": "TYPE_OF_TOPPING",
                            "confirmationRequired": false,
                            "elicitationRequired": true,
                            "prompts": {
                                "elicitation": "Elicit.Slot.1360006066067.416886211555"
                            }
                        }
                    ]
                }
            ],
            "delegationStrategy": "ALWAYS"
        },
        "prompts": [
            {
                "id": "Elicit.Slot.1218686242843.959247103465",
                "variations": [
                    {
                        "type": "PlainText",
                        "value": "Would you like a small, medium, large or extra large pizza?"
                    }
                ]
            },
            {
                "id": "Elicit.Slot.651250309317.169830581825",
                "variations": [
                    {
                        "type": "PlainText",
                        "value": "Would you like a thin crust, sicilian, deep dish or hand tossed"
                    }
                ]
            },
            {
                "id": "Elicit.Slot.1360006066067.416886211555",
                "variations": [
                    {
                        "type": "PlainText",
                        "value": "What type of topping do you want,  peperonni, ham, cheese or  sausage"
                    }
                ]
            }
        ]
    }
}
