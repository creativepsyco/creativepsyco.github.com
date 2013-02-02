---
layout: post
title: "Django Dynamic Forms"
date: 2013-02-02 20:33
comments: true
categories: [Django]
---

Here is an example of how to do a dynamic form in Django. I am loving this framework, it's so simple

```python
class ServiceStartForm(forms.Form):
    serviceList = service.models.Service.objects.all()
    print serviceList.count()
    b = {}
    for aService in serviceList:
        b[aService.id] = aService.name
    c = b.items()
    print "Within the form, ", serviceList.count()
    serviceChoice = forms.ChoiceField(choices=c, widget=forms.Select())
    input_directory = forms.CharField(max_length=200)
    output_directory = forms.CharField(max_length=200)

    def __init__(self, *args, **kwargs):
        super(ServiceStartForm, self).__init__(*args, **kwargs)
        serviceList = service.models.Service.objects.all()
        b = {}
        for aService in serviceList:
            b[aService.id] = aService.name
        c = b.items()
        self.fields["serviceChoice"].choices = c

```

Enjoy :)