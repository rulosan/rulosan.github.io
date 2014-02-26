---
layout: post
title:  "Consumiendo Event Kit para IOS 7"
categories: event kit ios7
---

# Objetivo

Lograr utilizar el Event Kit de IOS7 para poder generar un __Recordatorio__

# Explicación

Dentro del IOS 7 se encuentra un Framework denominando Event Kit el cual nos da la oportunidad de 
acceder tanto al __Calendario__ como a los __Recordatorios__ del telefono bajo el consentimiento del usuario

Esto nos sirve si queremos desde nuestra app algún recordario o alguna fecha de calendario que requiera
recordar, o en caso contrario hacer alguna consulta con sus recordarios ( lo cual veo un poco extraño )

# Prueba de concepto

* Tenemos que agregar en nuestro proyecto el __EventKit.framework__ en las propiedades de nuestro proyecto.
* Luego en el header file de nuestro Controller debemos de insertar el poderosisimo 

```

#import <UIKit/UIKit.h>

```
* El paso siguiente es pedirle permiso al usuario si quiere que nuestra app acceda a nuestros __Recordatorios__ ( tambien puedes acceder al __Calendario__ ver la documentacion de la manzanita).

```
/*
	MiRecordatorio.h
*/

//Declaramos como propiedad en la clase que la vayamos a usar
@property ( strong , nonatomic ) EKEventStore * eventStore;
```

* Ahora si viene lo bueno , pedimos permisos al usuario para acceder a sus __Recordatorios__


```
/* 
	Mi Recordatorio.m
*/

- ( void ) miMetodoParaPedirPermiso
{
	if (_eventStore == nil)
    {
        _eventStore = [[EKEventStore alloc] init];
        [_eventStore requestAccessToEntityType:EKEntityTypeReminder 
                                    completion:^(BOOL granted, NSError *error) {
            if( !granted )
            {
            	// granted nos confirma si el usuario garantizo el acceso a sus
            	// recordatorios
                NSLog(@" Acccess to reminders not granted");
            }
        }];
    }
    
    if( _eventStore != nil)
    {
        [self createRecordatorio];
    }
}
```

* Creamos el recordatorio y lo salvamos :)

```
- ( void ) createRecordatorio
{
    // Creamos un recordatorio
    EKReminder * reminder = [EKReminder reminderWithEventStore:self.eventStore];
    
    //Le asignamos el texto del textbox
    reminder.title = @"Estes es mi titulo";
    
    reminder.calendar = [_eventStore defaultCalendarForNewReminders];
    
    // En este caso estoy usuando un datePicker para capturar la fecha desde la UI
    NSDate * date = [_myDatePicker date];
    
    EKAlarm * alarm = [EKAlarm alarmWithAbsoluteDate:date];

    [reminder addAlarm:alarm];
    
    NSError * error = nil;
    
    // Lo salvamos y le decimos que le de commit , a final de cuentas los recordatorios
    // son una base de datos
    BOOL success = [_eventStore saveReminder:reminder commit:YES error:&error];
    
    if(success)
    {
        NSLog(@"el guardado fue exitoso");
    }

    if(error)
    {
        NSLog(@"hubo peditos al guardar el recodatorio %@",error);
    }
}

```

# Notas

Hasta el momento solo tengo entendido que no puedes generar nuevas listas de __Recordatorios__
por lo tanto todo recordatorio que se guarda aparecerá en la lista de pendientes.