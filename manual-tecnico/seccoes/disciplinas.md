---
description: Secção das disciplinas
---

# Disciplinas

### Modelo \(base de dados\)

Este modelo, apesar de simples, contém alguns detalhes importantes, como o método `get_absolute_url`, que permite criar um URL canónico para aceder aos objetos criados.

{% code title="models.py" %}
```python
...
class Disciplina(models.Model):
    utilizador = models.ForeignKey(User, on_delete=models.CASCADE)
    descricao = models.CharField(max_length=50, verbose_name="Descrição")
    professor = models.OneToOneField('Professor', on_delete=models.SET_NULL, blank=True, null=True, verbose_name="Professor")

    class Meta:
        verbose_name = "Disciplina"
        verbose_name_plural = "Disciplinas"

    def get_absolute_url(self):
        return reverse('app-disciplinas-editar', kwargs={'pk': self.pk})

    def __str__(self):
        return self.descricao
...
```
{% endcode %}

### Views \(vistas\)

Algumas...

{% code title="views\_app.py" %}
```python
...
class DisciplinaListView(LoginRequiredMixin, ListView):
    """
    Mostra a lista de disciplinas
    """

    model = Disciplina
    template_name = 'myschool_app/app/disciplinas.html'
    context_object_name = 'disciplinas'

    def get_queryset(self):
        return Disciplina.objects.filter(utilizador=self.request.user)


class DisciplinaCreateView(LoginRequiredMixin, SuccessMessageMixin, CreateView):
    """
    Cria uma disciplina
    """

    model = Disciplina
    template_name = 'myschool_app/app/disciplinas_form.html'
    fields = ['descricao', 'professor']
    success_url = reverse_lazy('app-disciplinas-lista')
    success_message = "A disciplina %(descricao)s foi criada com sucesso!"

    def form_valid(self, form):
        form.instance.utilizador = self.request.user
        return super().form_valid(form)


class DisciplinaUpdateView(LoginRequiredMixin, SuccessMessageMixin, UserPassesTestMixin, UpdateView):
    """
    Edita uma disciplina
    """

    model = Disciplina
    template_name = 'myschool_app/app/disciplinas_form.html'
    fields = ['descricao', 'professor']
    success_url = reverse_lazy('app-disciplinas-lista')
    context_object_name = 'disciplina'
    success_message = "A disciplina %(descricao)s foi atualizada com sucesso!"

    def form_valid(self, form):
        form.instance.utilizador = self.request.user
        return super().form_valid(form)

    def test_func(self):
        disciplina = self.get_object()
        if self.request.user == disciplina.utilizador:
            return True
        return False


class DisciplinaDeleteView(LoginRequiredMixin, SuccessMessageMixin, UserPassesTestMixin, DeleteView):
    """
    Remove uma disciplina
    """

    model = Disciplina
    template_name = 'myschool_app/app/disciplinas_remover.html'
    success_url = reverse_lazy('app-disciplinas-lista')
    context_object_name = 'disciplina'
    success_message = "A disciplina %(descricao)s foi removida com sucesso!"

    def test_func(self):
        disciplina = self.get_object()
        if self.request.user == disciplina.utilizador:
            return True
        return False

    def delete(self, request, *args, **kwargs):
        disciplina = self.get_object()
        messages.success(self.request, self.success_message %
                         disciplina.__dict__)
        return super(DisciplinaDeleteView, self).delete(request, *args, **kwargs)
...
```
{% endcode %}

### URL

URL's acessíveis pelos utilizadores da aplicação.

{% code title="urls\_app.py" %}
```python
...
path('disciplinas/', DisciplinaListView.as_view(), name="app-disciplinas-lista"),
path('disciplinas/adicionar/', DisciplinaCreateView.as_view(), name="app-disciplinas-adicionar"),
path('disciplinas/<int:pk>/editar/', DisciplinaUpdateView.as_view(), name="app-disciplinas-editar"),
path('disciplinas/<int:pk>/remover/', DisciplinaDeleteView.as_view(), name="app-disciplinas-remover"),
...
```
{% endcode %}

