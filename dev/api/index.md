---
layout: default
breadcrumbs: [{ "text": "API", "url": "/dev/api"}]
root: "../"
---

## API Endpoints

> <span>Announcement</span>
> We improved the API just for you! The main changes are:    
>   - Add language support    
>   - Use academicTerm instead of year    
>	- Key concepts with the same json structure (degree, course, space, period)    
>	- Spaces blueprints    
>	- Assigned room in evaluations    
>	- Add room exam and normal capacity    
>	- Add shift occupation in course schedule    
>
> You can see what changed in detail <a href="https://github.com/FenixEdu/fenixedu.github.io/commit/8502b4485d617d24f5f5faa484dd7e581b943e18">here</a>. 

This page essentially lists all the existing endpoints, as well as examples
when making invocations. While it is not the purpose of this page to describe 
the business entities exposed by this API, we will try to explain the meaning 
of each values whenever it is not self-evident. The current API exposes four
central concepts of the FenixEdu platform: people, spaces, degrees and courses.

Here, a space represents a resource such as a campus, a building, a building
level (floor) or a room. A degree represents a set of knowledge offered by a 
school or university, such that when a student completes the associated studies
he/she is eligible for a diploma and is often conferred a corresponding title.
A course is a concrete unit of teaching that typically lasts one academic term.

This documentation is applicable to all FenixEdu installations as of version 
1.2.0. Check your local installation to find out if the API is available. Be 
aware that some institutions may choose to restrict access to the API.

### Language Support

The API supports localized invocations.
In each endpoint if a `lang` parameter is present and its' value is an available language, the returned information is localized in the specified language. Otherwise the default language is used.

The list of available languages is returned by [/about](#get-about).

#### Example request with specified language

<a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612925989?lang=en-US">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612925989?lang=en-US</a>



### Public Endpoints
* [GET /about](#get-about) <i class="icon-lock-open"></i>
* [GET /academicterms](#get-academicterms) <i class="icon-lock-open"></i>
* [GET /canteen](#get-canteen) <i class="icon-lock-open"></i>
* [GET /contacts](#get-contacts) <i class="icon-lock-open"></i>
* [GET /courses/{id}](#get-courses-id) <i class="icon-lock-open"></i>
* [GET /courses/{id}/evaluations](#get-courses-id-evaluations) <i class="icon-lock-open"></i>
* [GET /courses/{id}/groups](#get-courses-id-groups) <i class="icon-lock-open"></i>
* [GET /courses/{id}/schedule](#get-courses-id-schedule) <i class="icon-lock-open"></i>
* [GET /courses/{id}/students](#get-courses-id-students) <i class="icon-lock-open"></i>
* [GET /degrees](#get-degrees) <i class="icon-lock-open"></i>
* [GET /degrees/{id}](#get-degrees-id) <i class="icon-lock-open"></i>
* [GET /degrees/{id}/courses](#get-degrees-id-courses) <i class="icon-lock-open"></i>
* [GET /domainModel](#get-domainmodel) <i class="icon-lock-open"></i>
* [GET /parking](#get-parking) <i class="icon-lock-open"></i>
* [GET /person](#get-person)  <i class="icon-lock"></i><i class="icon-user"></i>
* [GET /person/calendar/classes](#get-person-calendar-classes) <i class="icon-lock"></i><i class="icon-calendar"></i>
* [GET /person/calendar/evaluations](#get-person-calendar-evaluations) <i class="icon-lock"></i><i class="icon-calendar"></i>
* [GET /person/courses](#get-person-courses) <i class="icon-lock"></i><i class="icon-graduation-cap"></i>
* [GET /person/curriculum](#get-person-curriculum) <i class="icon-lock"></i><i class="icon-graduation-cap"></i>
* [GET /person/evaluations](#get-person-evaluations) <i class="icon-lock"></i><i class="icon-chart-area"></i>
* [PUT /person/evaluations/{id}](#put-person-evaluations-id) <i class="icon-lock"></i><i class="icon-chart-area"></i>
* [GET /person/payments](#get-person-payments) <i class="icon-lock"></i><i class="icon-basket"></i>
* [GET /shuttle](#get-shuttle)  <i class="icon-lock-open"></i>
* [GET /spaces](#get-spaces)  <i class="icon-lock-open"></i>
* [GET /spaces/{id}](#get-spaces-id)  <i class="icon-lock-open"></i>
* [GET /spaces/{id}/blueprint](#get-spaces-id-blueprint)  <i class="icon-lock-open"></i>

> <span>NOTE</span>
> <i class="icon-lock-open"></i> - Public Endpoint.      
> <i class="icon-lock"></i> - Private Endpoint that requires user authorization.      
> <i class="icon-user"></i> - Public personal information: name, emails, ist-id, photo and webpage.      
> <i class="icon-calendar"></i> - Information about the users's academic schedule and calendar.      
> <i class="icon-graduation-cap"></i> - Curricular information about both enrolling and teaching courses.      
> <i class="icon-chart-area"></i> - Curricular information about both enrolling and teaching courses.      
> <i class="icon-basket"></i> - Information about payments and debt.      

### GET /about

<i class="icon-lock-open"></i>

This endpoint returns some basic information about the institution where the 
application is deployed. It also returns a list of RSS feeds, the current academic term, available languages and default language.

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/about">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/about</a>

#### Example Response
{% highlight json %}
{
  "institutionName": "Instituto Superior Técnico",
  "institutionUrl": "",
  "rssFeeds": [
    {
      "description": "News",
      "url": ""
    },
    {
      "description": "Events",
      "url": ""
    }
  ],
  "currentAcademicTerm": "1ºSemestre 2013/2014",
  "languages": [
    "en-US",
    "pt-PT"
  ],
  "language": "pt-PT"
}
{% endhighlight %}

### GET /academicterms

<i class="icon-lock-open"></i>

This endpoint returns all the academic terms available to be used in other endpoints as academicTerm query parameter.
The returned object keys are not ordered in any particular way.

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/academicterms">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/academicterms</a>

#### Example Response
{% highlight json %}
{
  "2013/2014": [
    "2º Semestre 2013/2014",
    "1ºSemestre 2013/2014"
  ],
  "2012/2013": [
    "2 Semestre 2012/2013",
    "1 Semestre 2012/2013"
  ],
  "2011/2012": [
    "2 Semestre 2011/2012",
    "1 Semestre 2011/2012"
  ],
  "2010/2011": [
    "2 Semestre 2010/2011",
    "1 Semestre 2010/2011"
  ],
  "2009/2010": [
    "2 Semestre 2009/2010",
    "1 Semestre 2009/2010"
  ]
}
{% endhighlight %}

### GET /canteen

<i class="icon-lock-open"></i>

This endpoint returns the menu information of Alameda's canteen.

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/canteen">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/canteen</a>

#### Example Response
{% highlight json %}
{
  "en-GB": [
    {
      "day": "24/2/2014",
      "meal": [
        {
          "info": [
            {
              "menu": "Diet Menu",
              "name": "Grilled flounder with vegetables",
              "type": "Diet"
            },
            {
              "menu": "Macrobiotic Menu",
              "name": "Tofu salad",
              "type": "Macrobiotic"
            },
            {
              "menu": "Traditional Menu",
              "name": "Pork steak with spaghetti",
              "type": "Meat"
            },
            {
              "menu": "Traditional Menu",
              "name": "Fried flounder",
              "type": "Fish"
            },
            {
              "menu": "Traditional Menu",
              "name": "Soup of green beans and carrots",
              "type": "Soup"
            }
          ],
          "type": "Lunch"
        },
        {
          "info": [
            {
              "menu": "Macrobiotic Menu",
              "name": "Fried vegetables with rice",
              "type": "Macrobiotic"
            },
            {
              "menu": "Traditional Menu",
              "name": "Pork with pickles and olive",
              "type": "Meat"
            },
            {
              "menu": "Traditional Menu",
              "name": "Roasted red fish",
              "type": "Fish"
            },
            {
              "menu": "Traditional Menu",
              "name": "Julienne soup",
              "type": "Soup"
            }
          ],
          "type": "Dinner"
        }
      ]
    }
  ],
  "pt-PT": [
    {
      "day": "24/2/2014",
      "meal": [
        {
          "info": [
            {
              "menu": "Menú Dieta",
              "name": "Solha grelhada com legumes",
              "type": "Dieta"
            },
            {
              "menu": "Menú Macrobiótica",
              "name": "Salada de tofu",
              "type": "Macrobiótico"
            },
            {
              "menu": "Menú Tradicional",
              "name": "Bifinhos de porco com esparguete",
              "type": "Carne"
            },
            {
              "menu": "Menú Tradicional",
              "name": "Solha au meunier",
              "type": "Peixe"
            },
            {
              "menu": "Menú Tradicional",
              "name": "Sopa de feijão verde e cenoura",
              "type": "Sopa"
            }
          ],
          "type": "Almoço"
        },
        {
          "info": [
            {
              "menu": "Menú Macrobiótica",
              "name": "Pataniscas de legumes com arroz",
              "type": "Macrobiótico"
            },
            {
              "menu": "Menú Tradicional",
              "name": "Carne de porco à alentejana",
              "type": "Carne"
            },
            {
              "menu": "Menú Tradicional",
              "name": "Red fish assado",
              "type": "Peixe"
            },
            {
              "menu": "Menú Tradicional",
              "name": "Sopa juliana",
              "type": "Sopa"
            }
          ],
          "type": "Jantar"
        }
      ]
    }
  ]
}
{% endhighlight %}

### GET /contacts

<i class="icon-lock-open"></i>

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/contacts">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/contacts</a>

#### Example Response
{% highlight json %}
[
  {
    "name": "Campus Alameda",
    "fax": "+351218499242",
    "phone": "+351218417000",
    "email": "",
    "address": "Av. Rovisco Pais, 1",
    "postalCode": "1049-001 Lisboa",
    "workingHours": ""
  },
  {
    "name": "Direção de Serviços de Informática - Alameda",
    "fax": "",
    "phone": "+351218417506",
    "email": "dsi@tecnico.ulisboa.pt",
    "address": "",
    "postalCode": "",
    "workingHours": "9h30 - 12h30 / 14h00 - 16h30"
  }
]
{% endhighlight %}

### GET /courses/{id}

<i class="icon-lock-open"></i>

A course is a concrete unit of teaching that typically lasts one academic term.
This endpoint shows some information regarding a particular course. The same
course may be lectured simultaneously in multiple degrees during the same
academic term.

The "competences" field holds curricular information for each set of degrees in
which the course is lectured. Usually this information is the same for all
the associated degrees.

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612925989">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612925989</a>

#### Example Response
{% highlight json %}
{
  "acronym": "FInd3",
  "name": "Frio Industrial",
  "academicTerm": "1ºSemestre 2013/2014",
  "evaluationMethod": "1 trabalho de grupo, com avaliação ...",
  "numberOfAttendingStudents": 123,
  "announcementLink": "https://fenix.tecnico.ulisboa.pt/rss.do?boardId=123",
  "summaryLink": "https://fenix.tecnico.ulisboa.pt/publico/rss.do?summaryId=123",
  "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/FInd3",
  "competences": [
    {
      "id": "1313123123232",
      "program": "",
      "bibliographicReferences": [
        {
          "author": "Roriz L. et al",
          "reference": "Ed. Orion ",
          "title": "Climatização: concepção, e instalação",
          "year": "2006",
          "type": "MAIN",
          "url": null
        }
      ],
      "degrees": [
        {
          "id": "2761663977513",
          "name": "Mestrado em Engenharia e Gestão da Energia",
          "acronym": "MEGE"
        }
      ]
    }
  ],
  "teachers": [
    {
      "name": "John Doe",
      "istId": "ist112345",
      "mails": [
        "john.doe@ist.utl.pt"
      ],
      "urls": [
        "http://web.ist.utl.pt/ist112345/"
      ]
    }
  ]
}
{% endhighlight %}

### GET /courses/{id}/evaluations

<i class="icon-lock-open"></i>

An evaluation is a component of a course in which the teacher determines
the extent of the students understanding of the program. Current known
implementations of evaluations are: tests, exams, projects, online tests
and ad-hoc evaluations.

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612926005/evaluations">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612926005/evaluations</a>

#### Example Response
{% highlight json %}
[
  {
    "type": "TEST",
    "name": "Teste 1º Teste",
    "evaluationPeriod": {
      "start": "2013-10-26 09:00",
      "end": "2013-10-26 11:00"
    },
    "enrollmentPeriod": {
      "start": "2013-10-01 14:25:32",
      "end": "2013-10-24 17:52:44"
    },
    "isInEnrolmentPeriod": false,
    "rooms": [
      {
        "type": "ROOM",
        "id": "2448131362251",
        "name": "C01 - sala de aula",
        "description": "C01 - Pavilhão Central (Alameda)",
        "capacity": {
          "examCapacity": 56,
          "normalCapacity": 24
        }
      },
      {
        "type": "ROOM",
        "id": "2448131362449",
        "name": "C11 - Sala aula",
        "description": "C11 - Pavilhão Central (Alameda)",
        "capacity": {
          "examCapacity": 87,
          "normalCapacity": 12
        }
      }
    ]
  }
]
{% endhighlight %}


### GET /courses/{id}/groups

<i class="icon-lock-open"></i>

Groups are used in courses for a wide range of purposes. The most typical are
for creating teams of students for laboratories or projects. Some groups are 
shared among different courses. The enrolment of student groups may be atomic
or individual, and may be restricted to an enrolment period.

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612926005/groups">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612926005/groups</a>

#### Example Response
{% highlight json %}
[
  {
    "name": "Projeto de Avaliação de Projetos",
    "description": "Cada grupo de trabalho tem como objetivo...",
    "enrolmentPeriod": {
      "start": "2013-10-01 14:25:32",
      "end": "2013-10-24 17:52:44"
    },
    "enrolmentPolicy": "ATOMIC",
    "minimumCapacity": 1,
    "maximumCapacity": 3,
    "idealCapacity": 2,
    "associatedCourses": [
      {
        "name": "Matemática Computacional",
        "id": "1132132564548",
        "degrees": [
          {
            "name": "Licenciatura Bolonha em Engenharia de Telecomunicações e Informática",
            "acronym": "LERC",
            "id": "2761663971586"
          },
          {
            "name": "Mestrado Bolonha em Engenharia Informática e de Computadores - Alameda",
            "acronym": "MEIC-A",
            "id": "2761663977513"
          },
          {
            "name": "Mestrado Integrado em Engenharia Civil",
            "acronym": "MEC",
            "id": "2761663971466"
          }
        ]
      },
      {
        "name": "Mecânica Quantica",
        "id": "1132132564555",
        "degrees": [
          {
            "name": "Mestrado Integrado em Arquitectura",
            "acronym": "MA",
            "id": "2761663971465"
          },
          {
            "name": "Diploma de Estudos Avançados em Engenharia Informática e de Computadores",
            "acronym": "DEIC",
            "id": "2761663971783"
          }
        ]
      }
    ]
  }
]
{% endhighlight %}


### GET /courses/{id}/schedule

<i class="icon-lock-open"></i>

Each course is lectured during a specific set of intervals. These intervals
make up the lesson period for that course. Each course also has a curricular
load that specifies the time each student will expend with the course. Each 
shift is the possible schedule in which a student should enrol.

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612925989/schedule">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612925989/schedule</a>

#### Example Response
{% highlight json %}
{
  "lessonPeriods": [
    {
      "start": "2014-02-21 00:00:00",
      "end": "2014-04-12 00:00:00"
    },
    {
      "start": "2014-04-18 00:00:00",
      "end": "2014-05-29 00:00:00"
    }
  ],
  "courseLoads": [
    {
      "type": "TEORICA",
      "totalQuantity": 42,
      "unitQuantity": 1.5
    },
    {
      "type": "LABORATORIAL",
      "totalQuantity": 30,
      "unitQuantity": 3
    }
  ],
  "shifts": [
    {
      "name": "AED2T01",
      "types": [
        "TEORICA"
      ],
      "occupation": {
        "current": 120,
        "max": 120
      },
      "lessons": [
        {
          "start": "2014-02-21 10:00:00",
          "end": "2014-02-21 12:00:00",
          "room": {
            "type": "ROOM",
            "name": "Ga1",
            "id": "132115446846"
          }
        },
        {
          "start": "2014-03-21 10:00:00",
          "end": "2014-03-21 12:00:00",
          "room": {
            "type": "ROOM",
            "name": "Ga1",
            "id": "132115446846"
          }
        },
        {
          "start": "2014-05-21 10:00:00",
          "end": "2014-05-21 12:00:00",
          "room": {
            "type": "ROOM",
            "name": "Ga3",
            "id": "132115446847"
          }
        }
      ],
      "rooms": [
        {
          "type": "ROOM",
          "id": "132115446847",
          "name": "Ga3 - S. aula",
          "description": "Ga3 - Pavilhão Central (Alameda)",
          "capacity": {
            "normal": 80,
            "exam": 40
          }
        }
      ]
    },
    {
      "name": "AED2L03",
      "types": [
        "LABORATORIAL"
      ],
      "occupation": {
        "current": 79,
        "max": 95
      },
      "lessons": [
        {
          "start": "2014-02-23 10:00:00",
          "end": "2014-02-23 13:00:00",
          "room": {
            "type": "ROOM",
            "name": "F1",
            "id": "132115446844"
          }
        },
        {
          "start": "2014-03-23 10:00:00",
          "end": "2014-03-23 13:00:00",
          "room": {
            "type": "ROOM",
            "name": "F2",
            "id": "132115446843"
          }
        },
        {
          "start": "2014-05-23 10:00:00",
          "end": "2014-05-23 13:00:00",
          "room": {
            "type": "ROOM",
            "name": "F1",
            "id": "132115446844"
          }
        }
      ],
      "rooms": [
        {
          "type": "ROOM",
          "id": "2448131361685",
          "name": "F1 - Sala de aula",
          "description": "F1 - Pavilhão de Informática I (Alameda)",
          "capacity": {
            "normal": 60,
            "exam": 30
          }
        }
      ]
    }
  ]
}
{% endhighlight %}


### GET /courses/{id}/students

<i class="icon-lock-open"></i>

This endpoint lists all the students attending the specified course. For each
student it indicates the corresponding degree. The endpoint also returns the
number of students officially enroled in the course.

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612925989/students">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/courses/1610612925989/students</a>

#### Example Response
{% highlight json %}
{
  "enrolmentCount": 32,
  "attendingCount": 41,
  "students": [
    {
      "username": "ist1234",
      "degree": {
        "name": "Mestrado Bolonha em Engenharia Informática e de Computadores - Alameda",
        "acronym": "MEIC-A",
        "id": "2761663971475"
      }
    },
    {
      "username": "ist1236",
      "degree": {
        "name": "Mestrado Integrado em Arquitectura",
        "acronym": "MA",
        "id": "2761663971465"
      }
    }
  ]
}
{% endhighlight %}


### GET /degrees

<i class="icon-lock-open"></i>

This endpoint returns the information for all degrees.
If no academicTerm is defined it returns the degree information for the `currentAcademicTerm`.

#### Query Parameters
**academicTerm** - one of the academicTerms available at [/academicterms](#get-academicterms)

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/degrees?academicTerm=2013/2014">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/degrees?academicTerm=2013/2014</a>

#### Example Response
{% highlight json %}
[
  {
    "id": "2761663977513",
    "name": "Engenharia e Gestão da Energia",
    "acronym": "MEGE",
    "academicTerms": [
      "2013/2014",
      "2012/2013"
    ],
    "academicTerm": "2013/2014",
    "type": "BOLONHA_MASTER_DEGREE",
    "typeName": "Master Degree (MSc)",
    "url": "https://fenix.tecnico.ulisboa.pt/cursos/mege",
    "campus": [
      {
        "type": "CAMPUS",
        "id": "2465311230081",
        "name": "Alameda"
      }
    ],
    "info": {
      "description": "",
      "objectives": "",
      "designFor": "",
      "requisites": "",
      "profissionalExits": "",
      "history": "",
      "operationRegime": "",
      "gratuity": "",
      "links": ""
    },
    "teachers": [
      {
        "name": "John Doe",
        "istId": "ist11234",
        "mails": [
          "foo@ist.utl.pt",
          "bar@ist.utl.pt"
        ],
        "urls": []
      }
    ]
  }
]
{% endhighlight %}

### GET /degrees/{id}

<i class="icon-lock-open"></i>

This endpoint returns the information for the {id} degree.
If no academicTerm is defined it returns the degree information for the `currentAcademicTerm`.

#### Query Parameters
**academicTerm** - one of the academicTerms available at [/academicterms](#get-academicterms)

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/degrees/2761663977513?academicTerm=2013/2014">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/degrees/2761663977513?academicTerm=2013/2014</a>

#### Example Response
{% highlight json %}
{
  "id": "2761663977513",
  "name": "Engenharia e Gestão da Energia",
  "acronym": "MEGE",
  "academicTerms": [
    "2013/2014",
    "2012/2013"
  ],
  "currentAcademicTerm": "2013/2014",
  "type": "BOLONHA_MASTER_DEGREE",
  "typeName": "Master Degree (MSc)",
  "url": "https://fenix.tecnico.ulisboa.pt/cursos/mege",
  "campus": [
    {
      "type": "CAMPUS",
      "id": "2465311230081",
      "name": "Alameda"
    }
  ],
  "info": {
    "description": "",
    "objectives": "",
    "designFor": "",
    "requisites": "",
    "profissionalExits": "",
    "history": "",
    "operationRegime": "",
    "gratuity": "",
    "links": ""
  },
  "teachers": [
    {
      "name": "John Doe",
      "istId": "ist11234",
      "mails": [
        "foo@ist.utl.pt",
        "bar@ist.utl.pt"
      ],
      "urls": []
    }
  ]
}
{% endhighlight %}

### GET /degrees/{id}/courses

<i class="icon-lock-open"></i>

This endpoint returns the informations for a degree's courses.
If no academicTerm is defined it returns the degree information for the `currentAcademicTerm`.
#### Query Parameters
**academicTerm** - one of the academicTerms available at [/academicterms](#get-academicterms)   

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/degrees/2761663977513/courses?academicTerm=2013/2014">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/degrees/2761663977513/courses?academicTerm=2013/2014</a>

#### Example Response
{% highlight json %}
[
  {
    "acronym": "FInd3",
    "credits": "4.5",
    "name": "Frio Industrial",
    "id": "1610612925565",
    "academicTerm": "1ºSemestre 2013/2014"
  },
  {
    "acronym": "MFC3",
    "credits": "6.0",
    "name": "Mecânica de Fluídos Computacional",
    "id": "1610612925545",
    "academicTerm": "1ºSemestre 2013/2014"
  }
]
{% endhighlight %}

### GET /domainModel

<i class="icon-lock-open"></i>

This endpoint returns a representation of the domain model for the application.
While this information is returned in a JSON format, the concepts underlying 
the domain model can be found on the Fenix Framework site:
<a href="http://fenix-framework.github.io/DML.html">http://fenix-framework.github.io/DML.html</a>

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/domainModel">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/domainModel</a>

#### Example Response
{% highlight json %}
{
  "classes": [
    {
      "className": "net.sourceforge.fenixedu.domain.Shift",
      "interfaces": [],
      "modifiers": [],
      "slots": [
        {
          "type": "java.lang.Integer",
          "name": "lotacao",
          "modifiers": [],
          "options": []
        },
        {
          "type": "java.lang.String",
          "name": "nome",
          "modifiers": [],
          "options": []
        },
        {
          "type": "java.lang.String",
          "name": "comment",
          "modifiers": [],
          "options": []
        }
      ]
    }
  ],
  "relations": [
    {
      "name": "RootDomainObjectNonRegularTeachingService",
      "roles": [
        {
          "type": "net.sourceforge.fenixedu.domain.NonRegularTeachingService",
          "name": "nonRegularTeachingServices",
          "modifiers": [],
          "multiplicityLower": "0",
          "multiplicityUpper": "*"
        },
        {
          "type": "org.fenixedu.bennu.core.domain.Bennu",
          "name": "rootDomainObject",
          "modifiers": [],
          "multiplicityLower": "0",
          "multiplicityUpper": "1"
        }
      ]
    }
  ]
}
{% endhighlight %}

### GET /parking

<i class="icon-lock-open"></i>

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/parking">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/parking</a>

#### Example Response
{% highlight json %}
{
  "Alameda": {
    "address": "Avenida Rovisco Pais, 1, 1049-001 Lisboa", 
    "campus": "Alameda", 
    "description": "Parque Estacionamento do campus Alameda do IST", 
    "freeSlots": 624, 
    "latlng": "38.738137,-9.139135", 
    "name": "Alameda", 
    "total": 680, 
    "workingHours": "24h"
  }, 
  "Arco Do Cego": {
    "address": "Parque do Arco Cego, Avenida JoÃ£o CrisÃ³stomo, 1000-178 Lisboa", 
    "campus": "Alameda", 
    "description": "Parque Estacionamento do Arco do Cego", 
    "freeSlots": 62, 
    "latlng": "38.73640895,-9.14313902", 
    "name": "Arco do Cego", 
    "total": 70, 
    "workingHours": "24h"
  }
}
{% endhighlight %}

### GET /person

<i class="icon-lock"></i><i class="icon-user"></i>

This endpoint allows to access the current person information.

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person</a>

#### Example Response
{% highlight json %}
{
  "roles": [
    {
      "type": "TEACHER",
      "department": {
        "name": "Departamento de Engenharia Informática",
        "acronym": "DEI"
      }
    },
    {
      "type": "STUDENT",
      "registrations": [
        {
          "name": "Mestrado Bolonha em Engenharia Informática e de Computadores - Alameda",
          "acronym": "MEIC-A",
          "id": "2761663971475",
          "academicTerms": [
            "1ºSemestre 2013/2014",
            "2º Semestre 2013/2014"
          ]
        }
      ]
    },
    {
      "type": "ALUMNI",
      "concludedRegistrations": [
        {
          "name": "Licenciatura Bolonha em Engenharia Informática e de Computadores - Alameda",
          "acronym": "LEIC-A",
          "id": "2761663971474",
          "academicTerms": [
            "1 Semestre 2010/2011",
            "2 Semestre 2010/2011",
            "1 Semestre 2011/2012",
            "1 Semestre 2012/2013",
            "2 Semestre 2012/2013"
          ]
        }
      ]
    }
  ],
  "photo": null,
  "name": "John Doe",
  "gender": "MALE",
  "birthday": "21/11/1990",
  "username": "ist112345",
  "email": "john.doe@ist.utl.pt",
  "personalEmails": [
    "john.doe@ist.utl.pt"
  ],
  "workEmails": [],
  "webAddresses": [
    "http://web.ist.utl.pt/ist112345/"
  ],
  "workWebAddresses": [
    "http://web.ist.utl.pt/ist112345/"
  ]
}
{% endhighlight %}


### GET /person/calendar/classes

<i class="icon-lock"></i><i class="icon-schedule"></i>

This endpoint returns the user's class information. This information can be retrieved both in iCalendar and JSON formats.

#### Query Parameters

**format** - "calendar" (iCal format) or "json"

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/calendar/classes?format=json">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/calendar/classes?format=json</a>

#### Example Response
{% highlight json %}
{
  "academicTerm": "2013/2014",
  "events": [
    {
      "classPeriod": {
        "start": "18/09/2013 17:30",
        "end": "18/09/2013 19:00"
      },
      "location": [
        {
          "type": "ROOM",
          "name": "F4",
          "id": "2448131363674"
        }
      ],
      "title": "Gestão : Problemas",
      "course": {
        "acronym": "Ges5",
        "name": "Gestão",
        "academicTerm": "1ºSemestre 2013/2014",
        "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/ges5/",
        "id": "1610612925989"
      }
    },
    {
      "classPeriod": {
        "start": "28/10/2013 14:30",
        "end": "28/10/2013 15:30"
      },
      "location": [
        {
          "type": "ROOM",
          "name": "QA02.4",
          "id": "2448131363664"
        }
      ],
      "title": "Análise Complexa e Equações Diferenciais : Teórica",
      "course": {
        "acronym": "aced42",
        "name": "Análise Complexa e Equações Diferenciais",
        "academicTerm": "1º Semester 2013/2014",
        "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/aced42/",
        "id": "1610612925691"
      }
    }
  ]
}
{% endhighlight %}

### GET /person/calendar/evaluations

<i class="icon-lock"></i><i class="icon-graduation-cap"></i>

This endpoint returns the students's evaluations information. This information can be retrieved both in iCalendar and JSON formats.

#### Query Parameters

**format** - "calendar" (iCal format) or "json"

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/calendar/evaluations?format=json">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/calendar/evaluations?format=json</a>

#### Example Response
{% highlight json %}
{
  "academicTerm": "2013/2014",
  "events": [
    {
      "evaluationPeriod": {
        "start": "04/10/2013 00:57",
        "end": "04/10/2013 01:57"
      },
      "location": [],
      "title": "Inicio das inscrições para 2º Teste : Análise Complexa e Equações Diferenciais",
      "course": {
        "acronym": "aced42",
        "name": "Análise Complexa e Equações Diferenciais",
        "academicTerm": "1ºSemestre 2013/2014",
        "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/aced42/",
        "id": "1610612925691"
      }
    },
    {
      "evaluationPeriod": {
        "start": "06/11/2013 19:00",
        "end": "04/10/2013 21:00"
      },
      "location": [
        {
          "type": "ROOM",
          "name": "F2",
          "id": "2448131363664"
        }
      ],
      "title": "1º Teste : Gestão",
      "courses": [
        {
          "acronym": "Ges5",
          "name": "Gestão",
          "academicTerm": "1ºSemestre 2013/2014",
          "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/ges5/",
          "id": "1610612925989"
        }
      ]
    }
  ]
}
{% endhighlight %}

### GET /person/courses

<i class="icon-lock"></i><i class="icon-graduation-cap"></i>

This endpoint returns the user's course information.

#### Query Parameters
**academicTerm** - one of the academicTerms available at [/academicterms](#get-academicterms)X
If no academicTerm is defined it returns the degree information for the `currentAcademicTerm`.
#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/courses?academicTerm=2013/2014">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/courses?academicTerm=2013/2014</a>

#### Example Response
{% highlight json %}
{
  "enrolments": [
    {
      "id": "1610612926309",
      "acronym": "IAC4",
      "name": "Introdução à Arquitetura de Computadores",
      "academicTerm": "1ºSemestre 2013/2014",
      "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/iac4/2013-2014/1-semestre",
      "grade": null
    },
    {
      "id": "1610612925989",
      "acronym": "Ges5",
      "name": "Gestão",
      "academicTerm": "1ºSemestre 2013/2014",
      "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/ges5/2013-2014/1-semestre",
      "grade": null
    }
  ],
  "teaching": [
    {
      "id": "1610612926363",
      "acronym": "SE2",
      "name": "Sistemas Entre-Pares e Redes Sobrepostas",
      "academicTerm": "1ºSemestre 2013/2014",
      "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/SE2/2013-2014/1-semestre"
    }
  ]
}
{% endhighlight %}

### GET /person/curriculum

<i class="icon-lock"></i><i class="icon-graduation-cap"></i>

Complete curriculum (only for students)

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/curriculum">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/curriculum</a>

#### Example Response

{% highlight json %}
[
  {
    "degree": {
      "name": "Mestrado Bolonha em Engenharia Informática e de Computadores - Alameda",
      "acronym": "MEIC-A",
      "id": "2761663971475"
    },
    "start": "19/07/2012",
    "end": null,
    "credits": 7.5,
    "average": 10,
    "calculatedAverage": 10,
    "isFinished": false,
    "numberOfApprovedCourses": 1,
    "currentYear": 1,
    "approvedCourses": [
      {
        "course": {
          "name": "Unidade Curricular Aplicacional 1 (Língua Natural)",
          "id": "1610612905780",
          "acronym": "LN-2",
          "academicTerm": "1ºSemestre 2013/2014",
          "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/ln-2/2012-2013/1-semestre"
        },
        "grade": "10",
        "ects": 7.5
      }
    ]
  }
]
{% endhighlight %}


### GET /person/evaluations

<i class="icon-lock"></i><i class="icon-graduation-cap"></i>

This endpoint returns the student's written evaluation information.

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/evaluations">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/evaluations</a>

#### Example Response
{% highlight json %}
[
  {
    "id": "2512556536123",
    "type": "TEST",
    "name": "Teste 1º Teste",
    "evaluationPeriod": {
      "start": "15/11/2013 18:00",
      "end": "15/11/2013 21:00"
    },
    "isInEnrolmentPeriod": false,
    "enrollmentPeriod": {
      "start": "2013-11-07 15:00:25",
      "end": "2013-11-12 13:00:25"
    },
    "isEnrolled": true,
    "courses": [
      {
        "id": "1610612926408",
        "acronym": "GPI4",
        "name": "Gestão de Projectos Informáticos",
        "academicTerm": "1ºSemestre 2013/2014",
        "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/gpi4/2013-2014/1-semestre"
      }
    ],
    "rooms": [
      {
        "type": "ROOM",
        "id": "2448131363664",
        "name": "F2 - Sala de Aula"
      },
      {
        "type": "ROOM",
        "id": "2448131363667",
        "name": "FA1 - Anfiteatro"
      }
    ],
    "assignedRoom": {
      "type": "ROOM",
      "id": "2448131363674",
      "name": "F4 - Sala de Aula"
    }
  },
  {
    "id": "2512556536124",
    "type": "EXAM",
    "name": "Exame 1º Época",
    "evaluationPeriod": {
      "start": "10/01/2014 08:00",
      "end": "10/01/2014 11:00"
    },
    "isInEnrolmentPeriod": false,
    "enrollmentPeriod": {
      "start": "2013-12-20 17:00:22",
      "end": "2014-01-07 12:00:22"
    },
    "isEnrolled": false,
    "courses": [
      {
        "id": "1610612926115",
        "acronym": "ASof22",
        "name": "Arquitecturas de Software",
        "academicTerm": "1ºSemestre 2013/2014",
        "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/asof22/2013-2014/1-semestre"
      }
    ],
    "rooms": [],
    "assignedRoom": null
  }
]
{% endhighlight %}

### PUT /person/evaluations/{id}

<i class="icon-lock"></i><i class="icon-graduation-cap"></i>

This endpoint allows the student to enroll or disenroll from a written evaluation.

#### Query Parameters

**enrol** - "yes" or "no"

#### Example Request
```PUT``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/evaluations/2512556533022?enrol=yes">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/evaluations/2512556533022?enrol=yes</a>

#### Example Response

returns the same as the endpoint above.

### GET /person/payments

<i class="icon-lock"></i><i class="icon-basket"></i>

This endpoint returns user's payments information.

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/payments">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/person/payments</a>

#### Example Response
{% highlight json %}
{
  "completed": [
    {
      "amount": "12.34",
      "type": "CASH",
      "description": "Taxa de Secretaria e Seguro - 2012/2013",
      "date": "30/12/2002"
    }
  ],
  "pending": [
    {
      "description": "Propina",
      "paymentPeriod": {
        "start": "13/09/2013 00:00",
        "end": "31/12/2013 23:59"
      },
      "entity": "12345",
      "reference": "111 222 333",
      "amount": "1234.56"
    }
  ]
}
{% endhighlight %}

### GET /shuttle

<i class="icon-lock-open"></i>

This endpoint returns the shuttle information

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/shuttle">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/shuttle</a>

#### Example Response
{% highlight json %}
{
  "stations": [
    {
      "name": "Alameda",
      "address": "Avenida Manuel da Maia 36",
      "latlng": "38.736926,-9.136565"
    },
    {
      "name": "Cacém",
      "address": "Rua Elias Garcia 2",
      "latlng": "38.766274,-9.298728"
    }
  ],
  "date": [
    {
      "start": "15/09/2014",
      "end": "30/09/2014",
      "type": "weekday"
    },
    {
      "start": "01/09/2014",
      "end": "12/09/2014",
      "type": "holidays"
    }
  ],
  "trips": [
    {
      "type": "weekday",
      "stations": [
        {
          "hour": "07.15",
          "station": "Alameda"
        },
        {
          "hour": "07.25",
          "station": "Sete-Rios"
        },
        {
          "hour": "07.55",
          "station": "Taguspark"
        }
      ]
    },
    {
      "type": "holidays",
      "stations": [
        {
          "hour": "08.00",
          "station": "Alameda"
        },
        {
          "hour": "08.15",
          "station": "Sete-Rios"
        },
        {
          "hour": "08.40",
          "station": "Taguspark"
        }
      ]
    }
  ]
}
{% endhighlight %}

### GET /spaces

<i class="icon-lock-open"></i>

This endpoint returns the information about the campi.
 
#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/spaces">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/spaces</a>

#### Example Response
{% highlight json %}
[
  {
    "id": "2465311230082",
    "name": "Taguspark",
    "type": "CAMPUS"
  },
  {
    "id": "2465311230081",
    "name": "Alameda",
    "type": "CAMPUS"
  }
]
{% endhighlight %}


### GET /spaces/{id}

<i class="icon-lock-open"></i>

This endpoint returns information about the space for a given {id}, its contained and parent spaces. The {id} can be for any of these types: "CAMPUS", "BUILDING", "FLOOR" or "ROOM".

#### Query Parameters

**day** - "dd/mm/yyyy"

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/spaces/2448131363667?day=21/02/2014">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/spaces/2448131363667?day=21/02/2014</a>

#### Example Response
{% highlight json %}
{
  "type": "ROOM",
  "id": "2448131363667",
  "name": "FA1 - Anfiteatro",
  "containedSpaces": [],
  "parentSpace": {
    "type": "FLOOR",
    "id": "2723009268079",
    "name": "0"
  },
  "description": "FA1 - Pavilhão de Informática I (Alameda)",
  "capacity": {
    "normal": 93,
    "exam": 30
  },
  "events": [
    {
      "type": "LESSON",
      "start": "10:30",
      "end": "12:00",
      "weekday": "Sex",
      "day": "21/02/2014",
      "period": {
        "start": "21/02/2014 10:30",
        "end": "21/02/2014 12:00"
      },
      "info": "T",
      "course": {
        "id": "1610612946588",
        "acronym": "IAED7645",
        "name": "Introdução aos Algoritmos e Estruturas de Dados",
        "academicTerm": "2 Semestre 2013/2014",
        "url": "https://fenix.tecnico.ulisboa.pt/disciplinas/iaed7645/2013-2014/2-semestre"
      }
    }
  ]
}
{% endhighlight %}

### GET /spaces/{id}/blueprint

<i class="icon-lock-open"></i>

This endpoint returns the space's blueprint in the required format

#### Query Parameters

**format** - "jpeg" or "dwg"

#### Example Request
```GET``` <a href="https://fenix.tecnico.ulisboa.pt/api/fenix/v1/spaces/2448131360897/blueprint?format=jpeg">https://fenix.tecnico.ulisboa.pt/api/fenix/v1/spaces/2448131360897/blueprint?format=jpeg</a>

#### Example Response
response content-type : "application/dwg" or "image/jpg"

response content: raw image data
