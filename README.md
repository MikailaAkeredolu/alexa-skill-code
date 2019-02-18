#DAY ONE

const Alexa = require('ask-sdk-core');

const LaunchRequestHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'LaunchRequest';
    },
    handle(handlerInput) {
        const speechText = 'Welcome to Wise greeter, you can ask me to say hello to a friend of yours, by saying something like, say hello to Matt';
        return handlerInput.responseBuilder
            .speak(speechText)
            .reprompt(speechText)
            .getResponse();
    }
};
const HelloWorldIntentHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'IntentRequest'
            && handlerInput.requestEnvelope.request.intent.name === 'HelloWorldIntent';
    },
    handle(handlerInput) {
        const guestName = handlerInput.requestEnvelope.request.intent.slots.name.value;
        const speechText = getGreeting() + ` ${guestName} , ` +  getQuote() + " Also your lucky lotto numbers are " + generateArrayOfLottoNumbers();
        return handlerInput.responseBuilder
            .speak(speechText)
         .reprompt(speechText)
            .getResponse();
    }
};
const HelpIntentHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'IntentRequest'
            && handlerInput.requestEnvelope.request.intent.name === 'AMAZON.HelpIntent';
    },
    handle(handlerInput) {
        const speechText = 'You can say hello to me! How can I help?';

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

   function getGreeting(){
       var myDate = new Date();
       var hours = myDate.getUTCHours() -5;
       if(hours < 0){
           hours = hours + 24;
       }
       if(hours < 12){
           return "good morning";
       }else if(hours < 18){
           return " Good afternoon";
       }else{
           return " good evening";
       }
   } 

   let greetings = [
       
       "One, that chases two rats will catch none." + " <audio src='soundbank://soundlibrary/animals/amzn_sfx_rat_squeaks_01'/>",
       "As long as there is life, there is hope" + "<audio src='soundbank://soundlibrary/magic/amzn_sfx_fairy_melodic_chimes_01'/>",
       "For the eyes not to witness evil, the legs are the remedy" + " <audio src='soundbank://soundlibrary/human/amzn_sfx_person_running_01'/>",
        "A flowing stream, never looks back " + "<audio src='soundbank://soundlibrary/nature/amzn_sfx_stream_01'/>",
       ];
       
       function getQuote(){
           let rand = Math.floor((Math.random() * greetings.length) + 1);
           return greetings[rand];
       }
       
       
       function generateArrayOfLottoNumbers(){
           let result = [ ];
           let myArray = getLottoNumber();
           let StringResult;
           let finalRes = '';
           for(let x = 0; x <= 5; x++){
                result.push(getLottoNumber(myArray));
                StringResult = result.toString();
                finalRes = StringResult.replace(/,/g, '-');

           }
           return finalRes;
        }
        
        function getLottoNumber(){
           let rand = Math.floor((Math.random() * 99) + 1);
           return rand;
       }
   
exports.handler = Alexa.SkillBuilders.custom()
    .addRequestHandlers(
        LaunchRequestHandler,
        HelloWorldIntentHandler,
        HelpIntentHandler,
        CancelAndStopIntentHandler,
        SessionEndedRequestHandler,
        ) 
    .addErrorHandlers(
        ErrorHandler)
    .lambda();


#Interaction Model

{
    "interactionModel": {
        "languageModel": {
            "invocationName": "wise greeter",
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
                    "name": "HelloWorldIntent",
                    "slots": [
                        {
                            "name": "name",
                            "type": "LIST_OF_NAMES"
                        }
                    ],
                    "samples": [
                        "please greet {name}",
                        "greet my friend {name}",
                        "help me greet {name}",
                        "greet {name} for me",
                        "say a greeting to {name}",
                        "greet {name}",
                        "say hi to {name}",
                        "say hello to {name}"
                    ]
                },
                {
                    "name": "AMAZON.NavigateHomeIntent",
                    "samples": []
                }
            ],
            "types": [
                {
                    "name": "LIST_OF_NAMES",
                    "values": [
                        {
                            "name": {
                                "value": "mary"
                            }
                        },
                        {
                            "name": {
                                "value": "jane"
                            }
                        },
                        {
                            "name": {
                                "value": "john"
                            }
                        },
                        {
                            "name": {
                                "value": "mikaila"
                            }
                        },
                        {
                            "name": {
                                "value": "matt"
                            }
                        },
                        {
                            "name": {
                                "value": "mike"
                            }
                        },
                        {
                            "name": {
                                "value": "tariq"
                            }
                        }
                    ]
                }
            ]
        }
    }
}
