export enum ActionTypeEnum {
  APPROVE = 'Approve',
  REJECT = 'Reject'
}

type ApproveRejectPopupProps = {
  showPopup: boolean;
  setShowPopup: (flag: boolean) => void;
  actionType: ActionTypeEnum | null;
  actionRowData: any;
  refreshGrid: () => void;
};

const [actionType, setActionType] = useState<ActionTypeEnum | null>(null);

setActionType(ActionTypeEnum.APPROVE);
// or
setActionType(ActionTypeEnum.REJECT);


<h2>{props.actionType ? `${props.actionType} PBI Report` : 'PBI Report'}</h2>