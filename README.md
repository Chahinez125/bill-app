# bill-app
## Debugs
### Debugs #1 - Login
**Comportements attendus:** Si un administrateur remplit correctement les champs du Login, il devrait naviguer sur la page Dashboard

**Solution:** Le selector prenait l'input "testid" de l'employé et non de l'admin

containers/Login.js

```javascript
handleSubmitAdmin = e => {
    e.preventDefault()
    const user = {
        type: "Admin",
        email: e.target.querySelector(`input[data-testid="admin-email-input"]`).value,
        password: e.target.querySelector(`input[data-testid="admin-password-input"]`).value,
        status: "connected"
    }
}
```

### Debugs #2 - Bills/Sort Bills
**Comportements attendus:** Faire passer le test au vert en réparant la fonctionnalité.

**Solution:** Trier le tableau avec un sort dans la vue et ajouter une propriété accept à l'input file pour dire quels types de fichier il doit accepter.

views/BillsUI.js

```javascript
const rows = (data) => {
    return (data && data.length) ? data.map(bill => row(bill)).join("") : ""
    return (data && data.length) ? data
        .sort((currentBill, nextBill) => new Date(nextBill.date) - new Date(currentBill.date))
        .map(bill => row(bill)).join("") : ""
}
```

### Debugs #3 - Bills/Validation de l'upload d'un fichier
**Comportements attendus:** Empêcher la saisie d'un document qui a une extension différente de jpg, jpeg ou png au niveau du formulaire du fichier NewBill.js


**Solution:** Récupérer le type du fichier grâce à ```file.name.split('.')[1]``` et ensuite vérifier si l'extension du fichier est bien une extension valide avec une boucle et un tableau d'extension valide puis vider le champ si ce n'est pas l'extension désirée.

containers/NewBill.js

```javascript
const fileExtension = file.name.split('.')[1]

const validExtensions = [
    {
        name: 'png',
    },
    {
        name: 'jpg',
    },
    {
        name: 'jpeg',
    },
]

let isValidExtension = false

validExtensions.forEach((validExtension) => {
    if (fileExtension === validExtension.name) {
        isValidExtension = true
    }
})

if (!isValidExtension) {
    e.target.value = ''
}
```

### Debugs #4 - Dashboard
**Comportements attendus:** Pouvoir déplier plusieurs listes et consulter les tickets de chacune des deux listes.

**Solution:** N'appliquer l'eventListener que sur les bills dépliés.

containers/Dashboard.js

```javascript
bills.forEach(bill => {
    $(`#open-bill${bill.id}`).click((e) => this.handleEditTicket(e, bill, bills))
    $(`#status-bills-container${this.index} #open-bill${bill.id}`).click((e) => this.handleEditTicket(e, bill, bills));
})
```