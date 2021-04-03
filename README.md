@Injectable({
  providedIn: 'root'
})
export class CandidateService {


  constructor(private location:Location,makecanService:MakingcandidateService) { }

   getRandomIntInclusive(min:number, max:number) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min + 1)) + min;
  }


  getCandidates(): Observable<candidate[]> {
    const candidates = of(CANDIDATES)
    return candidates
  }
  getCandidatebyid(id:number): Observable<candidate> {
    return of(CANDIDATES.find(candidate => candidate.id === id))
  }
  removecandidate(candidate:candidate) {
    const remcan =  CANDIDATES.indexOf(candidate)
    if(-1 !== remcan) {
      CANDIDATES.splice(remcan, 1);

  }
  }
  makeCandidatesinpairs(): Observable<number> {
    const candidates = CANDIDATES
    let y: number = 1
    let x : number = candidates.length
    // "noImplicitReturns": false, to work
    while (x >= 2) {

        if (x == 2){
          x = Math.pow(x,y)
          console.log(x,"my num")
          return of(x)

        }
        x = x / 2
        y = y + 1
        if (x < 2 && 1 < x){
            x = (2 - x) + x
            x = Math.pow(x,y)
            console.log(x,"my new num")
            return of(x)

        }
        if (x <= 0){
           console.error("Write more candidates");
           this.location.back();
           break

        }
      }
  }
  makeafreecan( newcaw:candidate): void{

     newcaw.id = CANDIDATES.length + 1
      newcaw.isFreeSlot = true
      console.log(newcaw)
      CANDIDATES.push(newcaw)
     console.log(CANDIDATES,"CANDIDATES")

  }
  assignNumbertocandidates(x:number): Observable<number> {

    const Clen = CANDIDATES.length
    if (x == Clen){
      x = x/2
      console.log(x)
      return of(x)
    }
    if (x != Clen) {
      let w = 0
    const cangen =  x - Clen
    const name:string = "freecan"
    while (w < cangen){
    this.makeafreecan({name} as candidate)
    w = w + 1
    }
      x = x/2
     return of(x)

  }
  // Funkcje do sortowania kandydatów
  }
  sorttolists(x:number,newcandidates:candidate[],innercandidatelist:candidate[]): void{
    if(x == newcandidates.length-1) {
      innercandidatelist.push(newcandidates[newcandidates.length-1])
      newcandidates.pop()
    }
    else{
      newcandidates.push(newcandidates.splice(x, 1)[0]);
  innercandidatelist.push(newcandidates[(newcandidates.length-1)])
  newcandidates.pop()
  }
  }
  checkaproperfree(innercandidatelistx:candidate[],w:number)  {
    for(var value of innercandidatelistx) {
      if (value.isFreeSlot){
        w = w + 1
      }
    }
    if (w == 2){
      return 2
    }else {return 0}
  }
  makeacount(cansad:candidate[],freecanlistfortrue:candidate[],freecanlistforfalse:candidate[]): void{
    let innerlisttrue:candidate[] = []
    let innerlistfalse:candidate[] = []
    let x = 0
    for(var value of cansad) {
      let valueindex = cansad.indexOf(value)
      if (value.isFreeSlot){
       console.log(valueindex,"k")
       innerlisttrue.push(value)
       }
       if (value.isFreeSlot == false){
        console.log(valueindex,"k")
        innerlistfalse.push(value)
        }
  }
  while (x != innerlistfalse.length){
    this.sorttolists(0,innerlistfalse,freecanlistforfalse)

    }

  while (x != innerlisttrue.length){
  this.sorttolists(0,innerlisttrue,freecanlistfortrue)

  }
}
  makealistfordev(x:number): Observable<candidate[][]>{
   let outhercandidatelist:candidate[][] = []
   let newcandidatesfree:candidate[] = []
   let newcandidates:candidate[] = []
   let newcandidatesnormal = CANDIDATES.slice(0)

    this.makeacount(newcandidatesnormal,newcandidatesfree,newcandidates)
    console.log(newcandidates, "newcans")
    console.log(newcandidatesfree, "newfreecans")

    while (x > 0){

      let innercandidatelist:candidate[] = []
      let z = +this.getRandomIntInclusive(0,newcandidates.length - 1)
      let w = +this.getRandomIntInclusive(0,newcandidatesfree.length - 1)
      let h = +this.getRandomIntInclusive(0,newcandidates.length - 2)

      this.sorttolists(z,newcandidates,innercandidatelist)
      if (newcandidatesfree.length > 0) {
        this.sorttolists(w,newcandidatesfree,innercandidatelist)
      }else {
        this.sorttolists(h,newcandidates,innercandidatelist)
      }

      outhercandidatelist.push(innercandidatelist)

      x = x - 1
    }
    console.log(outhercandidatelist, "other")
    return of(outhercandidatelist)
  }
}


